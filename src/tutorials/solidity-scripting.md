## Solidity 脚本

### 简介

Solidity 脚本是一种使用 Solidity 声明式部署合约的方式，而不是使用更受限制且不太友好的 [`forge create`](../reference/forge/forge-create.md)。

Solidity 脚本类似于你在使用 Hardhat 等工具时编写的脚本；不同的是，它们是用 Solidity 而不是 JavaScript 编写的，并且它们在快速的 Foundry EVM 后端上运行，提供干运行功能。

### 高级概述

`forge script` 不是同步工作的。首先，它会收集脚本中的所有交易，然后才会广播它们。它基本上可以分为四个阶段：

1. 本地模拟 - 合约脚本在本地 EVM 中运行。如果提供了 rpc/fork URL，它将在该上下文中执行脚本。任何来自 `vm.broadcast` 和/或 `vm.startBroadcast` 的**外部调用**（非静态，非内部）将被附加到一个列表中。
2. 链上模拟 - 可选。如果提供了 rpc/fork URL，那么它将在此处顺序执行前一阶段收集的所有交易。
3. 广播 - 可选。如果提供了 `--broadcast` 标志并且前一阶段成功，它将广播在步骤 `1` 中收集并在步骤 `2` 中模拟的交易。
4. 验证 - 可选。如果提供了 `--verify` 标志，有 API 密钥，并且前一阶段成功，它将尝试验证合约（例如 Etherscan）。

鉴于这个流程，重要的是要注意到，其行为可能受外部状态/参与者影响的交易可能会有与步骤 `2` 中模拟的结果不同的结果。例如，抢先交易。

### 设置

让我们尝试使用 Solidity 脚本部署在 solmate 教程中制作的 NFT 合约。首先，我们需要通过以下方式创建一个新的 Foundry 项目：

```sh
forge init solidity-scripting
```

由于 solmate 教程中的 NFT 合约继承了 `solmate` 和 `OpenZeppelin` 合约，我们需要将它们作为依赖项安装：

```sh
# 进入项目
cd solidity-scripting

# 安装 Solmate 和 OpenZeppelin 合约作为依赖项
forge install transmissions11/solmate Openzeppelin/openzeppelin-contracts@v5.0.1
```

接下来，我们需要删除 `src` 文件夹中的 `Counter.sol` 文件，并创建另一个名为 `NFT.sol` 的文件。你可以通过运行以下命令来完成：

```sh
rm src/Counter.sol test/Counter.t.sol && touch src/NFT.sol && ls src
```

![设置命令](../images/solidity-scripting/set-up-commands.png)

完成后，你应该打开你喜欢的代码编辑器，并将下面的代码复制到 `NFT.sol` 文件中。

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.8.10;

import "solmate/tokens/ERC721.sol";
import "openzeppelin-contracts/contracts/utils/Strings.sol";
import "openzeppelin-contracts/contracts/access/Ownable.sol";

error MintPriceNotPaid();
error MaxSupply();
error NonExistentTokenURI();
error WithdrawTransfer();

contract NFT is ERC721, Ownable {

    using Strings for uint256;
    string public baseURI;
    uint256 public currentTokenId;
    uint256 public constant TOTAL_SUPPLY = 10_000;
    uint256 public constant MINT_PRICE = 0.08 ether;

    constructor(
        string memory _name,
        string memory _symbol,
        string memory _baseURI
    ) ERC721(_name, _symbol) Ownable(msg.sender) {
        baseURI = _baseURI;
    }

    function mintTo(address recipient) public payable returns (uint256) {
        if (msg.value != MINT_PRICE) {
            revert MintPriceNotPaid();
        }
        uint256 newTokenId = ++currentTokenId;
        if (newTokenId > TOTAL_SUPPLY) {
            revert MaxSupply();
        }
        _safeMint(recipient, newTokenId);
        return newTokenId;
    }

    function tokenURI(uint256 tokenId)
        public
        view
        virtual
        override
        returns (string memory)
    {
        if (ownerOf(tokenId) == address(0)) {
            revert NonExistentTokenURI();
        }
        return
            bytes(baseURI).length > 0
                ? string(abi.encodePacked(baseURI, tokenId.toString()))
                : "";
    }

    function withdrawPayments(address payable payee) external onlyOwner {
        uint256 balance = address(this).balance;
        (bool transferTx, ) = payee.call{value: balance}("");
        if (!transferTx) {
            revert WithdrawTransfer();
        }
    }
}
```

现在，让我们尝试编译我们的合约，确保一切正常。

```sh
forge build
```

如果你的输出看起来像这样，合约成功编译。
![编译成功](../images/solidity-scripting/compile-successful.png)

### 部署我们的合约

我们将把 `NFT` 合约部署到 Sepolia 测试网，但为此我们需要配置 Foundry，设置 Sepolia RPC URL、一个有 Sepolia Eth 资金的账户的私钥，以及一个用于验证 NFT 合约的 Etherscan 密钥。

> 💡 注意：你可以在这里获取一些 Sepolia 测试网 ETH [here](https://sepoliafaucet.com/)。

#### 环境配置

一旦你有了所有这些，创建一个 `.env` 文件并添加变量。Foundry 会自动加载项目目录中存在的 `.env` 文件。

.env 文件应遵循以下格式：

```sh
SEPOLIA_RPC_URL=
PRIVATE_KEY=
ETHERSCAN_API_KEY=
```

现在我们需要编辑 `foundry.toml` 文件。项目根目录中应该已经有一个。

在文件末尾添加以下行：

```toml
[rpc_endpoints]
sepolia = "${SEPOLIA_RPC_URL}"

[etherscan]
sepolia = { key = "${ETHERSCAN_API_KEY}" }
```

这为 Sepolia 创建了一个 [RPC 别名](../cheatcodes/rpc.md) 并加载了 Etherscan API 密钥。

#### 编写脚本

接下来，我们需要创建一个文件夹并命名为 `script`，并在其中创建一个名为 `NFT.s.sol` 的文件。这是我们将创建部署脚本的地方。

`NFT.s.sol` 的内容应如下所示：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Script.sol";
import "../src/NFT.sol";

contract MyScript is Script {
    function run() external {
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        vm.startBroadcast(deployerPrivateKey);

        NFT nft = new NFT("NFT_tutorial", "TUT", "baseUri");

        vm.stopBroadcast();
    }
}
```

现在让我们阅读代码并弄清楚它的实际含义和作用。

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;
```

记住，即使这是一个脚本，它仍然像一个智能合约一样工作，但从未部署，所以就像任何其他用 Solidity 编写的智能合约一样，必须指定 `pragma version`。

```solidity
import "forge-std/Script.sol";
import "../src/NFT.sol";
```

就像我们可能在编写测试时导入 Forge Std 以获取测试工具一样，Forge Std 也提供了一些脚本工具，我们在这里导入。

下一行只是导入 `NFT` 合约。

```solidity
contract MyScript is Script {
```

我们创建了一个名为 `MyScript` 的合约，并继承了 Forge Std 中的 `Script`。

```solidity
function run() external {
```

默认情况下，脚本通过调用名为 `run` 的函数来执行，这是我们的入口点。

```solidity
uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
```

这从我们的 `.env` 文件中加载私钥。**注意：** 在 `.env` 文件中暴露私钥并将其加载到程序中时必须小心。这仅推荐用于非特权部署者或在本地 / 测试设置中使用。对于生产设置，请查看 Foundry 支持的各种 [钱包选项](../reference/forge/forge-script.md#wallet-options---raw)。

```solidity
vm.startBroadcast(deployerPrivateKey);
```

这是一个特殊的作弊码，记录由我们的主脚本合约进行的调用和合约创建。我们传递 `deployerPrivateKey` 以指示它使用该密钥签署交易。稍后，我们将广播这些交易以部署我们的 NFT 合约。

```solidity
NFT nft = new NFT("NFT_tutorial", "TUT", "baseUri");
```

这里我们只是创建了我们的 NFT 合约。因为我们在这行之前调用了 `vm.startBroadcast()`，合约创建将被 Forge 记录，正如前面提到的，我们可以广播交易以在链上部署合约。广播交易日志将默认存储在 `broadcast` 目录中。你可以通过在 `foundry.toml` 文件中设置 [`broadcast`](../reference/config/project.md#broadcast) 来更改日志位置。

现在你已经了解了脚本智能合约的作用，让我们运行它。

你应该已经将前面提到的变量添加到 `.env` 文件中，以便下一部分能够工作。

在项目根目录运行：

```sh
# 加载 .env 文件中的变量
source .env

# 部署并验证我们的合约
forge script --chain sepolia script/NFT.s.sol:MyScript --rpc-url $SEPOLIA_RPC_URL --broadcast --verify -vvvv
```

Forge 将运行我们的脚本并为我们广播交易——这可能需要一段时间，因为 Forge 还会等待交易收据。一分钟左右后，你应该会看到类似这样的内容：

![合约已验证](../images/solidity-scripting/contract-verified.png)

这确认你已成功将 `NFT` 合约部署到 Sepolia 测试网，并在 Etherscan 上验证了它，所有这些都通过一个命令完成。

### 本地部署

你可以通过将端口配置为 `fork-url` 来部署到 Anvil，本地测试网。

在这里，我们在账户方面有两个选项。我们可以不带任何标志启动 anvil 并使用提供的私钥之一。或者，我们可以传递一个助记词给 anvil 使用。

#### 使用 Anvil 的默认账户

首先，启动 Anvil：

```sh
anvil
```

更新你的 `.env` 文件，使用 Anvil 提供给你的私钥。

然后运行以下脚本：

```sh
forge script script/NFT.s.sol:MyScript --fork-url http://localhost:8545 --broadcast
```

#### 使用自定义助记词

在你的 `.env` 文件中添加以下行，并用你的助记词完成：

```sh
MNEMONIC=
```

我们之前设置的 `PRIVATE_KEY` 环境变量应该是这个助记词中的前 10 个账户之一。

使用自定义助记词启动 Anvil：

```sh
source .env

anvil -m $MNEMONIC
```

然后运行以下脚本：

```sh
forge script script/NFT.s.sol:MyScript --fork-url http://localhost:8545 --broadcast
```

> 💡 注意：本教程的完整实现可以在这里找到 [here](https://github.com/Perelyn-sama/solidity-scripting)，关于 Solidity 脚本的进一步阅读，你可以查看 `forge script` [参考](../reference/forge/forge-script.md)。
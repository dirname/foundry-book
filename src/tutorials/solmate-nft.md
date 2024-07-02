## 使用 Solmate 创建 NFT

本教程将指导您使用 Foundry 和 [Solmate](https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC721.sol) 创建一个 OpenSea 兼容的 NFT。本教程的完整实现可以在此处找到 [这里](https://github.com/FredCoen/nft-tutorial)。

##### 本教程仅用于说明目的，并按原样提供。本教程未经过审计也未完全测试。本教程中的代码不应在生产环境中使用。

### 创建项目并安装依赖

首先按照 [入门部分](../getting-started/installation.html) 中概述的步骤设置一个 Foundry 项目。我们还将安装 Solmate 以获取其 ERC721 实现，以及一些 OpenZeppelin 实用库。通过从项目根目录运行以下命令来安装依赖项：

```bash
forge install transmissions11/solmate Openzeppelin/openzeppelin-contracts
```

这些依赖项将作为 git 子模块添加到您的项目中。

如果您正确地按照说明操作，您的项目结构应如下所示：

![项目结构](../images/nft-tutorial/nft-tutorial-project-structure.png)

### 实现一个基本的 NFT

然后，我们将样板合约 `src/Contract.sol` 重命名为 `src/NFT.sol` 并替换代码：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.10;

import "solmate/tokens/ERC721.sol";
import "openzeppelin-contracts/contracts/utils/Strings.sol";

contract NFT is ERC721 {
    uint256 public currentTokenId;

    constructor(
        string memory _name,
        string memory _symbol
    ) ERC721(_name, _symbol) {}

    function mintTo(address recipient) public payable returns (uint256) {
        uint256 newItemId = ++currentTokenId;
        _safeMint(recipient, newItemId);
        return newItemId;
    }

    function tokenURI(uint256 id) public view virtual override returns (string memory) {
        return Strings.toString(id);
    }
}
```

让我们来看看这个非常基本的 NFT 实现。我们首先从 git 子模块中导入两个合约。我们导入 Solmate 的 gas 优化 ERC721 标准实现，我们的 NFT 合约将继承自该实现。我们的构造函数接受 NFT 的 `_name` 和 `_symbol` 参数，并将它们传递给父 ERC721 实现的构造函数。最后，我们实现了 `mintTo` 函数，允许任何人铸造一个 NFT。该函数递增 `currentTokenId` 并使用父合约的 `_safeMint` 函数。

### 使用 forge 编译和部署

要编译 NFT 合约，请运行 `forge build`。您可能会遇到由于错误映射导致的构建失败：

```text
Error:
Compiler run failed
error[6275]: ParserError: Source "lib/openzeppelin-contracts/contracts/contracts/utils/Strings.sol" not found: File not found. Searched the following locations: "/PATH/TO/REPO".
 --> src/NFT.sol:5:1:
  |
5 | import "openzeppelin-contracts/contracts/utils/Strings.sol";
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

这可以通过设置正确的重映射来修复。在项目中创建一个 `remappings.txt` 文件并添加以下行：

```text
openzeppelin-contracts/=lib/openzeppelin-contracts/
```

（您可以在 [依赖文档](../projects/dependencies.md) 中了解更多关于重映射的信息。

默认情况下，编译器输出将在 `out` 目录中。要使用 Forge 部署我们编译的合约，我们必须为要使用的 RPC 端点和私钥设置环境变量。

通过运行以下命令设置环境变量：

```bash
export RPC_URL=<Your RPC endpoint>
export PRIVATE_KEY=<Your wallets private key>
```

设置完成后，您可以通过运行以下命令并添加相关的构造函数参数来使用 Forge 部署 NFT 合约：

```bash
forge create NFT --rpc-url=$RPC_URL --private-key=$PRIVATE_KEY --constructor-args <name> <symbol>
```

如果成功部署，您将看到部署钱包的地址、合约地址以及交易哈希打印到终端。

### 从您的合约中铸造 NFT

使用 Cast，Foundry 的命令行工具与智能合约交互、发送交易和获取链数据，可以简化在 NFT 合约上调用函数的过程。让我们看看如何使用它从我们的 NFT 合约中铸造 NFT。

鉴于您在部署期间已经设置了 RPC 和私钥环境变量，通过运行以下命令从您的合约中铸造一个 NFT：

```bash
cast send --rpc-url=$RPC_URL <contractAddress>  "mintTo(address)" <arg> --private-key=$PRIVATE_KEY
```

做得好！您刚刚从您的合约中铸造了第一个 NFT。您可以通过运行以下 `cast call` 命令来检查 NFT 的所有者，`currentTokenId` 等于 **1**。您提供的地址应作为所有者返回。

```bash
cast call --rpc-url=$RPC_URL --private-key=$PRIVATE_KEY <contractAddress> "ownerOf(uint256)" 1
```

### 扩展我们的 NFT 合约功能和测试

让我们通过添加元数据来表示 NFT 的内容，以及设置铸造价格、最大供应量和提取铸造收益的可能性来扩展我们的 NFT。要继续操作，您可以用下面的代码片段替换当前的 NFT 合约：

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
    ) ERC721(_name, _symbol) {
        baseURI = _baseURI;
    }

    function mintTo(address recipient) public payable returns (uint256) {
        if (msg.value != MINT_PRICE) {
            revert MintPriceNotPaid();
        }
        uint256 newTokenId = currentTokenId + 1;
        if (newTokenId > TOTAL_SUPPLY) {
            revert MaxSupply();
        }
        currentTokenId = newTokenId;
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
        if (address(this).balance == 0) {
            revert WithdrawTransfer();
        }
        
        payable(payee).transfer(address(this).balance);
    }

    function _checkOwner() internal view override {
        require(msg.sender == owner(), "Ownable: caller is not the owner");
    }
}
```

除了其他功能外，我们还添加了可以通过任何前端应用程序（如 OpenSea）查询的元数据，通过调用 NFT 合约上的 `tokenURI` 方法。

> **注意**：如果您想在部署时提供一个真实的 URL 到构造函数，并托管此 NFT 合约的元数据，请按照 [此处](https://docs.opensea.io/docs/part-3-upload-metadata) 概述的步骤操作。

让我们测试一些添加的功能，以确保它们按预期工作。Foundry 通过 Forge 提供了一个极快的 EVM 原生测试框架。

在您的测试文件夹中，将当前的 `Contract.t.sol` 测试文件重命名为 `NFT.t.sol`。该文件将包含所有关于 NFT 的 `mintTo` 方法的测试。接下来，用以下内容替换现有的样板代码：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.10;

import "forge-std/Test.sol";
import "../src/NFT.sol";

contract NFTTest is Test {
    using stdStorage for StdStorage;

    NFT private nft;

    function setUp() public {
        // Deploy NFT contract
        nft = new NFT("NFT_tutorial", "TUT", "baseUri");
    }

    function test_RevertMintWithoutValue() public {
        vm.expectRevert(MintPriceNotPaid.selector);
        nft.mintTo(address(1));
    }

    function test_MintPricePaid() public {
        nft.mintTo{value: 0.08 ether}(address(1));
    }

    function test_RevertMintMaxSupplyReached() public {
        uint256 slot = stdstore
            .target(address(nft))
            .sig("currentTokenId()")
            .find();
        bytes32 loc = bytes32(slot);
        bytes32 mockedCurrentTokenId = bytes32(abi.encode(10000));
        vm.store(address(nft), loc, mockedCurrentTokenId);
        vm.expectRevert(MaxSupply.selector);
        nft.mintTo{value: 0.08 ether}(address(1));
    }

    function test_RevertMintToZeroAddress() public {
        vm.expectRevert("INVALID_RECIPIENT");
        nft.mintTo{value: 0.08 ether}(address(0));
    }

    function test_NewMintOwnerRegistered() public {
        nft.mintTo{value: 0.08 ether}(address(1));
        uint256 slotOfNewOwner = stdstore
            .target(address(nft))
            .sig(nft.ownerOf.selector)
            .with_key(address(1))
            .find();

        uint160 ownerOfTokenIdOne = uint160(
            uint256(
                (vm.load(address(nft), bytes32(abi.encode(slotOfNewOwner))))
            )
        );
        assertEq(address(ownerOfTokenIdOne), address(1));
    }

    function test_BalanceIncremented() public {
        nft.mintTo{value: 0.08 ether}(address(1));
        uint256 slotBalance = stdstore
            .target(address(nft))
            .sig(nft.balanceOf.selector)
            .with_key(address(1))
            .find();

        uint256 balanceFirstMint = uint256(
            vm.load(address(nft), bytes32(slotBalance))
        );
        assertEq(balanceFirstMint, 1);

        nft.mintTo{value: 0.08 ether}(address(1));
        uint256 balanceSecondMint = uint256(
            vm.load(address(nft), bytes32(slotBalance))
        );
        assertEq(balanceSecondMint, 2);
    }

    function test_SafeContractReceiver() public {
        Receiver receiver = new Receiver();
        nft.mintTo{value: 0.08 ether}(address(receiver));
        uint256 slotBalance = stdstore
            .target(address(nft))
            .sig(nft.balanceOf.selector)
            .with_key(address(receiver))
            .find();

        uint256 balance = uint256(vm.load(address(nft), bytes32(slotBalance)));
        assertEq(balance, 1);
    }

    function test_RevertUnSafeContractReceiver() public {
        // Adress set to 11, because first 10 addresses are restricted for precompiles
        vm.etch(address(11), bytes("mock code"));
        vm.expectRevert(bytes(""));
        nft.mintTo{value: 0.08 ether}(address(11));
    }

    function test_WithdrawalWorksAsOwner() public {
        // Mint an NFT, sending eth to the contract
        Receiver receiver = new Receiver();
        address payable payee = payable(address(0x1337));
        uint256 priorPayeeBalance = payee.balance;
        nft.mintTo{value: nft.MINT_PRICE()}(address(receiver));
        // Check that the balance of the contract is correct
        assertEq(address(nft).balance, nft.MINT_PRICE());
        uint256 nftBalance = address(nft).balance;
        // Withdraw the balance and assert it was transferred
        nft.withdrawPayments(payee);
        assertEq(payee.balance, priorPayeeBalance + nftBalance);
    }

    function test_WithdrawalFailsAsNotOwner() public {
        // Mint an NFT, sending eth to the contract
        Receiver receiver = new Receiver();
        nft.mintTo{value: nft.MINT_PRICE()}(address(receiver));
        // Check that the balance of the contract is correct
        assertEq(address(nft).balance, nft.MINT_PRICE());
        // Confirm that a non-owner cannot withdraw
        vm.expectRevert("Ownable: caller is not the owner");
        vm.startPrank(address(0xd3ad));
        nft.withdrawPayments(payable(address(0xd3ad)));
        vm.stopPrank();
    }
}

contract Receiver is ERC721TokenReceiver {
    function onERC721Received(
        address operator,
        address from,
        uint256 id,
        bytes calldata data
    ) external override returns (bytes4) {
        return this.onERC721Received.selector;
    }
}

```

测试套件设置为一个合约，其中 `setUp` 方法在每个单独的测试之前运行。

如您所见，Forge 提供了许多 [作弊码](../cheatcodes/) 来操纵状态以适应您的测试场景。

例如，我们的 `testFailMaxSupplyReached` 测试检查当 NFT 的最大供应量达到时尝试铸造是否失败。因此，需要使用存储作弊码将 NFT 合约的 `currentTokenId` 设置为最大供应量，该作弊码允许您写入合约的存储槽。可以使用 [`forge-std`](https://github.com/foundry-rs/forge-std/) 帮助库轻松找到要写入的存储槽。您可以使用以下命令运行测试：

```bash
forge test
```

如果您想将 Forge 技能付诸实践，请为 NFT 合约的其余方法编写测试。欢迎将它们 PR 到 [nft-tutorial](https://github.com/FredCoen/nft-tutorial)，您将在其中找到本教程的完整实现。

### 函数调用的 Gas 报告

Foundry 提供了关于合约的综合 Gas 报告。对于测试中的每个函数调用，它返回最小、平均、中位数和最大 Gas 成本。要打印 Gas 报告，只需运行：

```bash
forge test --gas-report
```

这在查看合约中的各种 Gas 优化时非常方便。

让我们看看通过将 OpenZeppelin 替换为 Solmate 进行 ERC721 实现所节省的 Gas。您可以在此处找到使用这两个库的 NFT 实现 [这里](https://github.com/FredCoen/nft-tutorial)。在运行 `forge test --gas-report` 时，以下是该存储库的 Gas 报告结果。

如您所见，我们使用 Solmate 的实现在成功铸造时节省了大约 500 Gas（`mintTo` 函数调用的最大 Gas 成本）。

![Gas report solmate NFT](../images/nft-tutorial/gas-report-solmate-nft.png)

![Gas report OZ NFT](../images/nft-tutorial/gas-report-oz-nft.png)

就是这样，我希望这能为您提供一个很好的实践基础，了解如何开始使用 Foundry。我们认为，没有比在 Solidity 中编写测试更好的方式来深入理解 Solidity 了。您还将体验到在 JavaScript 和 Solidity 之间切换的上下文切换更少。祝您编码愉快！

> 注意：请按照 [此教程](./solidity-scripting.md) 学习如何使用 Solidity 脚本部署此处使用的 NFT 合约。
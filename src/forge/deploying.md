## 部署

Forge 可以使用 [`forge create`](../reference/forge/forge-create.md) 命令将智能合约部署到指定的网络。

Forge CLI 一次只能部署一个合约。

如果要一次性部署和验证多个智能合约，Forge 的 [Solidity 脚本](../tutorials/solidity-scripting.md#deploying-our-contract) 会是更高效的方法。

要部署合约，必须提供一个 RPC URL（环境变量：`ETH_RPC_URL`）和部署合约的账户的私钥。

将 `MyContract` 部署到一个网络：

```sh
$ forge create --rpc-url <your_rpc_url> --private-key <your_private_key> src/MyContract.sol:MyContract
compiling...
success.
Deployer: 0xa735b3c25f...
Deployed to: 0x4054415432...
Transaction hash: 0x6b4e0ff93a...
```

Solidity 文件可能包含多个合约。上面的 `:MyContract` 指定了从 `src/MyContract.sol` 文件中部署哪个合约。

使用 `--constructor-args` 标志向构造函数传递参数：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

import {ERC20} from "solmate/tokens/ERC20.sol";

contract MyToken is ERC20 {
    constructor(
        string memory name,
        string memory symbol,
        uint8 decimals,
        uint256 initialSupply
    ) ERC20(name, symbol, decimals) {
        _mint(msg.sender, initialSupply);
    }
}
```

此外，我们可以通过传递 `--verify` 让 Forge 在 Etherscan、Sourcify 或 Blockscout 上验证我们的合约（如果网络支持）。

```sh
$ forge create --rpc-url <your_rpc_url> \
    --constructor-args "ForgeUSD" "FUSD" 18 1000000000000000000000 \
    --private-key <your_private_key> \
    --etherscan-api-key <your_etherscan_api_key> \
    --verify \
    src/MyToken.sol:MyToken
```

## 验证已存在的合约

建议在 `forge create` 时使用 `--verify` 标志，以便在部署后自动在浏览器上验证合约。
注意，对于 Etherscan，必须设置 [`ETHERSCAN_API_KEY`](../reference/config/etherscan.md#etherscan_api_key)。

如果你要验证已部署的合约，请继续阅读。

你可以使用 [`forge verify-contract`](../reference/forge/forge-verify-contract.md) 命令在 Etherscan、Sourcify、oklink 或 Blockscout 上验证合约。

必须提供：
- 合约地址
- 合约名称或合约路径 `<path>:<contractname>`
- 你的 Etherscan API 密钥（环境变量：`ETHERSCAN_API_KEY`）（如果在 Etherscan 上验证）。

此外，你可能需要提供：
- 构造函数参数的 ABI 编码格式（如果有）
- 用于构建的 [编译器版本](https://etherscan.io/solcversions)，带有从提交版本前缀的 8 位十六进制数字（提交通常不是 nightly 构建）。如果未指定，则会自动检测。
- 优化次数，如果激活了 Solidity 优化器。如果未指定，则会自动检测。
- [链 ID](https://evm-chainlist.netlify.app/)，如果合约不在以太坊主网上

假设你要验证上面的 `MyToken`。你将 [优化次数](../reference/config/solidity-compiler.md#optimizer_runs) 设置为 100 万，使用 v0.8.10 编译，并部署到 Sepolia 测试网（链 ID：11155111）。注意，如果在验证时未设置 `--num-of-optimizations`，则默认为 0，而在部署时默认为 200，因此如果你使用默认编译设置，请确保传递 `--num-of-optimizations 200`。

以下是如何验证它：

```bash
forge verify-contract \
    --chain-id 11155111 \
    --num-of-optimizations 1000000 \
    --watch \
    --constructor-args $(cast abi-encode "constructor(string,string,uint256,uint256)" "ForgeUSD" "FUSD" 18 1000000000000000000000) \
    --etherscan-api-key <your_etherscan_api_key> \
    --compiler-version v0.8.10+commit.fc410830 \
    <the_contract_address> \
    src/MyToken.sol:MyToken 

Submitted contract for verification:
                Response: `OK`
                GUID: `a6yrbjp5prvakia6bqp5qdacczyfhkyi5j1r6qbds1js41ak1a`
                url: https://sepolia.etherscan.io//address/0x6a54…3a4c#code
```

建议在 `verify-contract` 命令中使用 [`--watch`](../reference/forge/forge-verify-contract.md#verify-contract-options) 标志，以便轮询验证结果。

如果未提供 `--watch` 标志，可以使用 [`forge verify-check`](../reference/forge/forge-verify-check.md) 命令检查验证状态：

```bash
$ forge verify-check --chain-id 11155111 <GUID> <your_etherscan_api_key>
Contract successfully verified.
```

<br>

> 💡 **提示**
> 
> 使用 Cast 的 [`abi-encode`](../reference/cast/cast-abi-encode.md) 来 ABI 编码参数。
>
> 在这个例子中，我们运行了 `cast abi-encode "constructor(string,string,uint8,uint256)" "ForgeUSD" "FUSD" 18 1000000000000000000000` 来 ABI 编码参数。

<br>

### 故障排除

##### `missing hex prefix ("0x") for hex string`

确保私钥字符串以 `0x` 开头。

##### `EIP-1559 not activated`
EIP-1559 在 RPC 服务器上不受支持或未激活。传递 `--legacy` 标志以使用传统交易而不是 EIP-1559 交易。如果你在本地环境中进行开发，可以使用 Hardhat 而不是 Ganache。

##### `Failed to parse tokens`
确保传递的参数类型正确。

##### `Signature error`
确保私钥正确。

##### `Compiler version commit for verify`
如果你想检查本地运行的确切提交，尝试：` ~/.svm/0.x.y/solc-0.x.y --version`，其中 `x` 和 `y` 分别是主版本号和次版本号。输出将类似于：

```ignore
solc, the solidity compiler commandline interface
Version: 0.8.12+commit.f00d7308.Darwin.appleclang
```

注意：你不能直接粘贴整个字符串 "0.8.12+commit.f00d7308.Darwin.appleclang" 作为编译器版本的参数。但你可以使用提交的 8 位十六进制数字来查找 [编译器版本](https://etherscan.io/solcversions) 中应复制和粘贴的内容。

### 已知问题

#### 验证具有模糊导入路径的合约

Forge 将源目录（`src`、`lib`、`test` 等）作为 `--include-path` 参数传递给编译器。
这意味着给定以下项目树
```text
|- src
|-- folder
|--- Contract.sol
|--- IContract.sol
```
可以在 `Contract.sol` 中使用 `folder/IContract.sol` 导入路径导入 `IContract`。

Etherscan 无法重新编译此类源代码。考虑将导入路径更改为相对导入路径。

#### 验证没有字节码哈希的合约

目前，无法在 Etherscan 上验证 [`bytecode_hash`](../reference/config/solidity-compiler.md#bytecode_hash) 设置为 `none` 的合约。
点击 [这里](https://docs.soliditylang.org/en/v0.8.13/metadata.html#usage-for-source-code-verification) 了解更多关于元数据哈希如何用于源代码验证的信息。
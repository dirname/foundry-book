## `deployCodeTo`

### 签名

```solidity
function deployCodeTo(string memory what, address where) internal virtual;
```

```solidity
function deployCodeTo(string memory what, bytes memory args, address where) internal virtual;
```

```solidity
function deployCodeTo(string memory what, bytes memory args, uint256 value, address where) internal virtual;
```

### 描述

通过从 artifacts 目录中获取合约字节码，伪部署合约到一个任意地址。这可以用于重新创建生产环境。

calldata 参数可以是以下形式之一：`ContractFile.sol`（如果文件名和合约名相同）、`ContractFile.sol:ContractName`，或者是相对于项目根目录的 artifact 路径。

还可以通过使用 `value` 参数传递值。如果你需要在构造时发送 ETH，这是必要的。

### 示例

```solidity
deployCodeTo("MyContract.sol", abi.encode(arg1, arg2), arbitraryAddr);
```

### 参见

Forge 标准库

- [`deployCode`](./deployCode.md)
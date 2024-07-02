## `deployCode`

### 签名

```solidity
function deployCode(string memory what) public returns (address);
```

```solidity
function deployCode(string memory what, bytes memory args) public returns (address);
```

```solidity
function deployCode(string memory what, uint256 val) public returns (address);
```

```solidity
function deployCode(string memory what, bytes memory args, uint256 val) public returns (address);
```

### 描述

通过从 artifacts 目录中获取合约字节码来部署合约。

calldata 参数可以是以下形式之一：`ContractFile.sol`（如果文件名和合约名相同）、`ContractFile.sol:ContractName`，或者是相对于项目根目录的 artifact 路径。

还可以通过使用 `val` 参数传递值。如果你需要在构造时发送 ETH，这是必要的。

### 示例

```solidity
address deployment = deployCode("MyContract.sol", abi.encode(arg1, arg2));
```

### 另请参阅

Forge 标准库

- [`deployCodeTo`](./deployCodeTo.md)
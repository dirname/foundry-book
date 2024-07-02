## `createSelectFork`

### 签名

```solidity
function createSelectFork(string calldata urlOrAlias) external returns (uint256);
```

```solidity
function createSelectFork(string calldata urlOrAlias, uint256 block) external returns (uint256);
```

```solidity
function createSelectFork(string calldata urlOrAlias, bytes32 transaction) external returns (uint256);
```

### 描述

从给定的端点创建并选择一个新的分叉，并返回分叉的标识符。如果传递了一个区块号作为参数，分叉将从该区块开始，否则它将从最新的区块开始。

如果提供了交易哈希，它将把分叉回滚到交易被挖出的区块，并重放所有之前执行的交易。

### 示例

创建并选择一个新的主网分叉，使用最新的区块号：

```solidity
uint256 forkId = vm.createSelectFork(MAINNET_RPC_URL);

assertEq(block.number, 15_171_037); // 截至撰写时，2022-07-19 04:55:27 UTC
```

创建并选择一个新的主网分叉，使用给定的区块号：

```solidity
uint256 forkId = vm.createSelectFork(MAINNET_RPC_URL, 1_337_000);

assertEq(block.number, 1_337_000);
```

### 另请参阅

- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
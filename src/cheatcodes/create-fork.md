## `createFork`

### 签名

```solidity
// 创建一个新的分叉，使用给定的端点和最新的区块，并返回分叉的标识符
function createFork(string calldata urlOrAlias) external returns (uint256)
```

```solidity
// 创建一个新的分叉，使用给定的端点和区块，并返回分叉的标识符
function createFork(string calldata urlOrAlias, uint256 block) external returns (uint256);
```

```solidity
// 创建一个新的分叉，使用给定的端点和给定交易所在的区块，并重放该区块中在该交易之前的所有交易
function createFork(string calldata urlOrAlias, bytes32 transaction) external returns (uint256);
```

### 描述

从给定的端点创建一个新的分叉，并返回分叉的标识符。如果传递了区块号作为参数，分叉将从该区块开始，否则将从最新的区块开始。

如果提供了交易哈希，它将把分叉回滚到交易所在的区块，并重放该区块中在该交易之前执行的所有交易。

### 示例

创建一个新的主网分叉，使用最新的区块号：

```solidity
uint256 forkId = vm.createFork(MAINNET_RPC_URL);
vm.selectFork(forkId);

assertEq(block.number, 15_171_037); // 截至撰写时，2022-07-19 04:55:27 UTC
```

创建一个新的主网分叉，使用给定的区块号：

```solidity
uint256 forkId = vm.createFork(MAINNET_RPC_URL, 1_337_000);
vm.selectFork(forkId);

assertEq(block.number, 1_337_000);
```

### 另请参阅

- [activeFork](./active-fork.md)
- [selectFork](./select-fork.md)
- [createSelectFork](./create-select-fork.md)
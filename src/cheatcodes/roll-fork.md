## `rollFork`

### 签名

```solidity
// 将_active_分叉滚动到给定的区块
function rollFork(uint256 blockNumber) external;
```

```solidity
// 将_active_分叉滚动到交易被挖出的区块，并重放所有之前执行的交易
function rollFork(bytes32 transaction) external;
```

```solidity
// 与`rollFork(uint256 blockNumber)`相同，但使用对应于`forkId`的分叉
function rollFork(uint256 forkId, uint256 blockNumber) external;
```

```solidity
// 与`rollFork(bytes32 transaction)`相同，但使用对应于`forkId`的分叉
function rollFork(uint256 forkId, bytes32 transaction) external;
```

### 描述

设置 `block.number`。如果传递了分叉标识符作为参数，它将更新该分叉，否则将更新当前活动的分叉。

如果提供了交易哈希，它将把分叉滚动到交易被挖出的区块，并重放所有之前执行的交易。

### 示例

为当前活动的分叉设置 `block.number`：

```solidity
uint256 forkId = vm.createFork(MAINNET_RPC_URL);
vm.selectFork(forkId);

assertEq(block.number, 15_171_037); // 截至撰写时，2022-07-19 04:55:27 UTC

vm.rollFork(15_171_057);

assertEq(block.number, 15_171_057);
```

为传递的 `forkId` 参数标识的分叉设置 `block.number`：

```solidity
uint256 optimismForkId = vm.createFork(OPTIMISM_RPC_URL);

vm.rollFork(optimismForkId, 1_337_000);

vm.selectFork(optimismForkId);

assertEq(block.number, 1_337_000);
```

### 参见

- [roll](./roll.md)
- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
- [activeFork](./active-fork.md)
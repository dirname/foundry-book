## `selectFork`

### 签名

```solidity
function selectFork(uint256 forkId) external;
```

### 描述

接受一个由 `createFork` 创建的分叉标识符，并将相应的分叉状态设置为活动状态。

### 示例

选择一个之前创建的分叉：

```solidity
uint256 forkId = vm.createFork(MAINNET_RPC_URL);

vm.selectFork(forkId);

assertEq(vm.activeFork(), forkId);
```

### 参见

- [createFork](./create-fork.md)
- [activeFork](./active-fork.md)
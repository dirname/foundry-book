## `activeFork`

### 签名

```solidity
function activeFork() external returns (uint256);
```

### 描述

返回当前活动分叉的标识符。如果没有当前活动的分叉，则回退。

### 示例

获取当前活动的分叉 ID：

```solidity
uint256 mainnetForkId = vm.createFork(MAINNET_RPC_URL);
uint256 optimismForkId = vm.createFork(OPTIMISM_RPC_URL);

assert(mainnetForkId != optimismForkId);

vm.selectFork(mainnetForkId);
assertEq(vm.activeFork(), mainnetForkId);

vm.selectFork(optimismForkId);
assertEq(vm.activeFork(), optimismForkId);
```

### 参见

- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
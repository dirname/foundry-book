## `getBlockTimestamp`

### 签名

```solidity
function getBlockTimestamp() external view returns (uint256 timestamp);
```

### 描述

获取当前的 `block.timestamp`。这在使用 `vm.warp` 和 `--via-ir` 编译的情况下非常有用，因为 `block.timestamp` 在交易过程中被假定为常量。这意味着在每次 forge 测试中，对 `block.timestamp` 的多次调用会被优化为仅返回一个常量值，而不是实际访问当前的 `block.timestamp`。`vm.getBlockTimestamp()` 避免了这种优化，并返回当前的 `block.timestamp`。

### 示例

```solidity
assertEq(vm.getBlockTimestamp(), 1, "timestamp 应该是 1");
vm.warp(10);
assertEq(vm.getBlockTimestamp(), 10, "warp 失败");
```
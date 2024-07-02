## `getBlockNumber`

### 签名

```solidity
function getBlockNumber() external view returns (uint256 timestamp);
```

### 描述

获取当前的 `block.number`。这在使用 `vm.roll` 和 `--via-ir` 编译的情况下非常有用，因为 `block.number` 在交易过程中被假定为常量。这意味着在每次 forge 测试中，多次调用 `block.number` 会被优化为仅返回一个常量值，而不是实际访问当前的 `block.number`。`vm.getBlockNumber()` 避免了这种优化，并返回当前的 `block.number`。

### 示例

```solidity
uint256 height = vm.getBlockNumber();
assertEq(height, uint256(block.number));
vm.roll(10);
assertEq(vm.getBlockNumber(), 10);
```
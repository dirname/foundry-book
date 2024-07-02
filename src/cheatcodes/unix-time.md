## `unixTime`

### 签名

```solidity
function unixTime() external returns (uint milliseconds);
```

### 描述

返回自 Unix 纪元以来的时间（以毫秒为单位）。

### 示例

```solidity
uint start = vm.unixTime();
vm.sleep(10_000); // 暂停执行 10 秒
uint end = vm.unixTime();
assertEq(end - start, 10_000);
```
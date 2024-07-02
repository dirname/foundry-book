## `record`

### 签名

```solidity
function record() external;
```

### 描述

告诉虚拟机开始记录所有的存储读取和写入。要访问这些读取和写入，请使用 [`accesses`](./accesses.md)。

> ℹ️ **注意**
>
> 每次写入也会计为一次额外的读取。

### 示例

```solidity
/// contract NumsContract {
///     uint256 public num1 = 100; // slot 0
///     uint256 public num2 = 200; // slot 1
/// }

vm.record();
numsContract.num2();
(bytes32[] memory reads, bytes32[] memory writes) = vm.accesses(
  address(numsContract)
);
emit log_uint(uint256(reads[0])); // 1
```

### 参见

Forge 标准库

[Std Storage](../reference/forge-std/std-storage.md)
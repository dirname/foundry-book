## `store`

### 签名

```solidity
function store(address account, bytes32 slot, bytes32 value) external;
```

### 描述

将值 `value` 存储在账户 `account` 的存储槽 `slot` 中。

### 示例

```solidity
/// contract LeetContract {
///     uint256 private leet = 1337; // slot 0
/// }

vm.store(address(leetContract), bytes32(uint256(0)), bytes32(uint256(31337)));
bytes32 leet = vm.load(address(leetContract), bytes32(uint256(0)));
emit log_uint(uint256(leet)); // 31337
```

### 参见

Forge 标准库

[Std Storage](../reference/forge-std/std-storage.md)
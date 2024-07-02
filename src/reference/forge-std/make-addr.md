## `makeAddr`

### 签名

```solidity
function makeAddr(string memory name) internal returns(address addr);
```

### 描述

从提供的 `name` 创建一个地址。

为派生的地址创建一个 [`label`](../../cheatcodes/label.md)，使用提供的 `name` 作为标签值。

### 示例

```solidity
address alice = makeAddr("alice");
emit log_address(alice); // 0x328809bc894f92807417d2dad6b7c998c1afdac6
```
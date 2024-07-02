## `prank`

### 签名

```solidity
function prank(address) external;
```

```solidity
function prank(address sender, address origin) external;
```

### 描述

将 `msg.sender` 设置为指定的地址 **仅对下一次调用** 有效。"下一次调用" 包括静态调用，但不包括对作弊码地址的调用。

如果使用 `prank` 的替代签名，则 `tx.origin` 也会在下一次调用中被设置。

### 示例

```solidity
/// function withdraw() public {
///     require(msg.sender == owner);

vm.prank(owner);
myContract.withdraw(); // [PASS]
```

### 另请参阅

Forge 标准库

[`hoax`](../reference/forge-std/hoax.md)
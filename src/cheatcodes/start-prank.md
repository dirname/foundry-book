## `startPrank`

### 签名

```solidity
function startPrank(address) external;
```

```solidity
function startPrank(address sender, address origin) external;
```

### 描述

设置 `msg.sender` **对于所有后续调用**，直到调用 [`stopPrank`](./stop-prank.md) 为止。

如果使用 `startPrank` 的替代签名，则 `tx.origin` 也会被设置为所有后续调用。

### 另请参阅

Forge 标准库

[`startHoax`](../reference/forge-std/startHoax.md), [`changePrank`](../reference/forge-std/change-prank.md)
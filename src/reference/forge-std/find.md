## `find`

### 签名

```solidity
function find(StdStorage storage self) internal returns (uint256);
```

### 描述

根据 [`target`](../forge-std/target.md)、[`sig`](../forge-std/sig.md)、[`with_key`](../forge-std/with_key.md)(s) 和 [`depth`](../forge-std/depth.md) 查找任意存储槽。

如果查找失败，则会回退并带有相应的消息。
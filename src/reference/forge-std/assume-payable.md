## `assumePayable`

### 签名

```solidity
function assumePayable(address addr) public;
```

### 描述

使用 [`assume`](../../cheatcodes/assume.md) 过滤拒绝以太币转账的地址。

这会向指定的 `addr` 地址发起一个没有调用数据的外部调用，并检查调用的成功与否。
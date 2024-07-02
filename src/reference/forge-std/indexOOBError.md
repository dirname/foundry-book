## `indexOOBError`

### 签名

```solidity
stdError.indexOOBError
```

### 描述

当尝试访问数组中越界的元素时，Solidity 内部抛出的错误。

对于外部合约中的空数组，此方法将不起作用。对于这些情况，请使用不带任何参数的 `expectRevert`。
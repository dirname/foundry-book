## `enumConversionError`

### 签名

```solidity
stdError.enumConversionError
```

### 描述

当尝试将一个数字转换为枚举的一个变体时，如果该数字大于枚举中的变体数量（从0开始计数），则会出现内部 Solidity 错误。
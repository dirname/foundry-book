## `bound`

### 签名

```solidity
function bound(uint256 x, uint256 min, uint256 max) public returns (uint256 result);
```

### 描述

一个用于将模糊测试输入包装到特定范围的数学函数。

在某些情况下，您可以使用它来代替 `assume` 作弊码以获得更好的性能。更多信息请阅读[这里](../../cheatcodes/assume.md)。

### 示例

```solidity
input = bound(input, 99, 101);
```

对于输入 `0`，返回 `99`。
<br>
对于输入 `1`，返回 `100`。
<br>
对于输入 `2`，返回 `101`。
<br>
对于输入 `3`，返回 `99`。
<br>
依此类推。
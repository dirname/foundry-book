## `assertApproxEqAbsDecimal`

### 签名

```solidity
function assertApproxEqAbsDecimal(uint256 left, uint256 right, uint256 maxDelta, uint256 decimals) internal;
```

```solidity
function assertApproxEqAbsDecimal(uint256 left, uint256 right, uint256 maxDelta, uint256 decimals, string memory err) internal;
```

```solidity
function assertApproxEqAbsDecimal(int256 left, int256 right, uint256 maxDelta, uint256 decimals) internal;
```

```solidity
function assertApproxEqAbsDecimal(int256 left, int256 right, uint256 maxDelta, uint256 decimals, string memory err) internal;
```

### 描述

断言 `left` 在绝对值的 delta 范围内近似等于 `right`。

`left` 和 `right` 在回退字符串中以小数格式显示。

可选地在回退字符串中包含错误消息。

### 另请参阅

- [`assertApproxEqAbs`](./assertApproxEqAbs.md)
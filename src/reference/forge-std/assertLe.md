## `assertLe`

### 签名

```solidity
function assertLe(uint256 left, uint256 right) internal;
```

```solidity
function assertLe(uint256 left, uint256 right, string memory err) internal;
```

```solidity
function assertLe(int256 left, int256 right) internal;
```

```solidity
function assertLe(int256 left, int256 right, string memory err) internal;
```

### 描述

断言 `left` 小于或等于 `right`。

可选地包含一个错误消息在回退字符串中。

### 另请参阅

- [`assertLeDecimal`](./assertLeDecimal.md)
- [`assertLt`](./assertLt.md)
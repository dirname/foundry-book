## `assertTrue`

### 签名

```solidity
function assertTrue(bool data) internal;
```

```solidity
function assertTrue(bool data, string memory err) internal;
```

### 描述

断言 `data` 为真。

可选地包含一个错误消息在回退字符串中。

### 示例

```solidity
bool success = contract.fun();
assertTrue(success);
```

### 另请参阅

- [`assertFalse`](./assertFalse.md)
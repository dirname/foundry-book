## `assertFalse`

### 签名

```solidity
function assertFalse(bool data) internal;
```

```solidity
function assertFalse(bool data, string memory err) internal;
```

### 描述

断言 `data` 为假。

可选地包含一个错误消息在回退字符串中。

### 示例

```solidity
bool failure = contract.fun();
assertFalse(failure);
```

### 另请参阅

- [`assertTrue`](./assertTrue.md)
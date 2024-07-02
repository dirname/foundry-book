## 标准日志

标准日志扩展了来自[`DSTest`](../ds-test.md#logging)库的日志事件。

### 事件

```solidity
event log_array(uint256[] val);
event log_array(int256[] val);
event log_named_array(string key, uint256[] val);
event log_named_array(string key, int256[] val);
```

### 用法

本节提供使用示例。

#### log\_array

```solidity
event log_array(<type>[] val);
```

其中 `<type>` 可以是 `int256`、`uint256`、`address`。

##### 示例

```solidity
// 假设存储
// uint256[] data = [10, 20, 30, 40, 50]; 

emit log_array(data);
```

#### log\_named\_array

```solidity
event log_named_array(string key, <type>[] val);
```

其中 `<type>` 可以是 `int256`、`uint256`、`address`。

##### 示例

```solidity
// 假设存储
// uint256[] data = [10, 20, 30, 40, 50]; 

emit log_named_array("Data", data);
```
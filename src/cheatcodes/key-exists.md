## `keyExists`

### 状态

`keyExists` 正在被 `keyExistsJson` 取代，未来版本中将被移除。

### 签名

```solidity
// 检查 JSON 字符串中是否存在某个键。
vm.keyExists(string memory json, string memory key) returns (bool)
```

### 描述

检查 JSON 字符串中是否存在某个键。

### 示例

```solidity
string memory path = "./path/to/jsonfile.json";
string memory json = vm.readFile(path);
bool exists = vm.keyExists(json, ".key");
assertTrue(exists);
```
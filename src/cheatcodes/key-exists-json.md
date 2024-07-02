## `keyExistsJson`

### 签名

```solidity
// 检查 JSON 字符串中是否存在某个键。
vm.keyExistsJson(string memory json, string memory key) returns (bool)
```

### 描述

检查 JSON 字符串中是否存在某个键。

### 示例

```solidity
string memory path = "./path/to/jsonfile.json";
string memory json = vm.readFile(path);
bool exists = vm.keyExistsJson(json, ".key");
assertTrue(exists);
```
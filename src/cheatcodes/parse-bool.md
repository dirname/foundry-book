## `parseBool`

### 签名

```solidity
function parseBool(string calldata stringifiedValue) external pure returns (bool parsedValue);
```

### 描述

将 `string` 类型的值解析为 `bool` 类型。

### 示例

```solidity
string memory boolAsString = "false";
bool stringToBool = vm.parseBool(boolAsString); // false
```
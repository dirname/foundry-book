## `parseInt`

### 签名

```solidity
function parseInt(string calldata stringifiedValue) external pure returns (int256 parsedValue);
```

### 描述

将 `string` 类型的值解析为 `int256` 类型。

### 示例

```solidity
string memory intAsString = "-12345";
int256 stringToInt = vm.parseInt(intAsString); // -12345
```
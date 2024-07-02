## `parseBytes32`

### 签名

```solidity
function parseBytes32(string calldata stringifiedValue) external pure returns (bytes32 parsedValue);
```

### 描述

将 `string` 类型的值解析为 `bytes32` 类型。

### 示例

```solidity
string memory bytes32AsString = "0x0000000000000000000000000000000000000000000000000000000000000000";
bytes32 stringToBytes32 = vm.parseBytes32(bytes32AsString); // 0x0000000000000000000000000000000000000000000000000000000000000000
```
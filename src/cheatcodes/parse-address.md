## `parseAddress`

### 签名

```solidity
function parseAddress(string calldata stringifiedValue) external pure returns (address parsedValue);
```

### 描述

将 `string` 类型的值解析为 `address` 类型。

### 示例

```solidity
string memory addressAsString = "0x0000000000000000000000000000000000000000";
address stringToAddress = vm.parseAddress(addressAsString); // 0x0000000000000000000000000000000000000000
```
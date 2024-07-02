## `getLabel`

### 签名

```solidity
function getLabel(address) external returns (string memory);
```

### 描述

如果之前已经标记了地址，则检索该地址的标签。如果没有标记，则返回前缀为 `unlabeled:` 的地址。
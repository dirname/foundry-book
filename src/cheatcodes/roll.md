## `roll`

### 签名

```solidity
function roll(uint256) external;
```

### 描述

设置 `block.number`。

### 示例

```solidity
vm.roll(100);
emit log_uint(block.number); // 100
```

### 另请参阅

- [rollFork](./roll-fork.md)
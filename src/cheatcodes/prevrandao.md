## `prevrandao`

### 签名

```solidity
function prevrandao(bytes32) external;
```

### 描述

设置 `block.prevrandao`。

如果在巴黎硬分叉之前的 EVM 版本中使用，将会回滚。在这种情况下，请使用 [`vm.difficulty`][prevrandao] 代替。

### 示例

```solidity
vm.prevrandao(bytes32(uint256(42)));
emit log_uint(block.prevrandao); // 42
```


[prevrandao]: ./difficulty.md
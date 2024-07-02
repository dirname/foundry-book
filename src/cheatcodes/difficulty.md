## `difficulty`

### 签名

```solidity
function difficulty(uint256) external;
```

### 描述

设置 `block.difficulty`。

如果在合并后的 EVM 版本（巴黎及以后）中使用，将会回滚。在这种情况下，请使用 [`vm.prevrandao`][prevrandao] 代替。

### 示例

```solidity
vm.difficulty(25);
emit log_uint(block.difficulty); // 25
```


[prevrandao]: ./prevrandao.md
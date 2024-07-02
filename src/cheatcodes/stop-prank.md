## `stopPrank`

### 签名

```solidity
function stopPrank() external;
```

### 描述

停止由 [`startPrank`](./start-prank.md) 启动的活跃恶作剧，将 `msg.sender` 和 `tx.origin` 重置为调用 `startPrank` 之前的值。
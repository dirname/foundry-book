## `startStateDiffRecording`

### 签名

```solidity
function startStateDiffRecording()
```

### 描述

记录所有作为 CREATE、CALL 或 SELFDESTRUCT 操作码一部分的状态变化，并按顺序记录调用上下文。
有关如何访问和解释记录的状态变化的更多详细信息，请参阅 [`stopAndReturnStateDiff`](./stop-and-return-state-diff.md)。
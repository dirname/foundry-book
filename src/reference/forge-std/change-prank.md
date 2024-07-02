## `changePrank`

### 状态

`changePrank` 已被弃用。

### 签名

```solidity
function changePrank(address who) internal;
```

### 描述

停止当前的恶作剧（prank）并使用 `stopPrank`，然后将地址传递给 `startPrank`。

这对于在 `setUp` 函数中启动全局恶作剧并在某些测试中停用它非常有用。
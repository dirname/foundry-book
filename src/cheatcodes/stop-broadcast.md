## `stopBroadcast`

### 签名
```solidity
function stopBroadcast() external;
```

### 描述

停止收集交易以便稍后在链上广播。

### 示例

```solidity
function deployNoArgs() public {
    // 广播下一个调用
    cheats.broadcast();
    Test test1 = new Test();

    // 广播此行和 `stopBroadcast` 之间的所有调用
    cheats.startBroadcast();
    Test test2 = new Test();
    cheats.stopBroadcast();
}
```
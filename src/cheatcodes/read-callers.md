## `readCallers`

### 签名

```solidity
enum CallerMode {
    None,
    Broadcast,
    RecurrentBroadcast,
    Prank,
    RecurrentPrank
}

function readCallers() 
external 
returns (CallerMode callerMode, address msgSender, address txOrigin);
```

### 描述

读取当前的 `CallerMode`、`msg.sender` 和 `tx.origin`。

`CallerMode` 枚举指示是否有活动的调用者修改及其类型。

- 如果有活动的 prank：
  - `callerMode` 将等于：
    - `CallerMode.Prank` 如果 prank 已通过 [`prank`](./prank.md) 设置。
    - `CallerMode.RecurrentPrank` 如果 prank 已通过 [`startPrank`](./start-prank.md) 设置。

- 如果有活动的 broadcast：
  - `callerMode` 将等于：
    - `CallerMode.Broadcast` 如果 broadcast 已通过 [`broadcast`](./broadcast.md) 设置。
    - `CallerMode.RecurrentBroadcast` 如果 broadcast 已通过 [`startBroadcast`](./start-broadcast.md) 设置。

- 如果没有活动的调用者修改：
  - `callerMode` 将等于 `CallerMode.None`。

### 示例

```solidity
CallerMode callerMode;
address msgSender;
address txOrigin;

// 示例 1
(callerMode, msgSender, txOrigin) = vm.readCallers();
assertEq(callerMode, CallerMode.None);
assertEq(msgSender, defaultSenderAddress);
assertEq(txOrigin, defaultOriginAddress);

// 示例 2
vm.prank(senderPrankAddress);
(callerMode, msgSender, txOrigin) = vm.readCallers();
assertEq(callerMode, CallerMode.Prank);
assertEq(msgSender, senderPrankAddress);
assertEq(txOrigin, defaultOriginAddress);

// 示例 3
vm.prank(senderPrankAddress, originPrankAddress);
(callerMode, msgSender, txOrigin) = vm.readCallers();
assertEq(callerMode, CallerMode.Prank);
assertEq(msgSender, senderPrankAddress);
assertEq(txOrigin, originPrankAddress);

// 示例 4
vm.broadcast(broadcastAddress);
(callerMode, msgSender, txOrigin) = vm.readCallers();
assertEq(callerMode, CallerMode.Broadcast);
assertEq(msgSender, broadcastAddress);
assertEq(txOrigin, broadcastAddress);
```

### 参见

- [prank](./prank.md)
- [startPrank](./start-prank.md)
- [broadcast](./broadcast.md)
- [startBroadcast](./start-broadcast.md)
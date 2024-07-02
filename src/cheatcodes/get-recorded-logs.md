## `getRecordedLogs`

### 签名

```solidity
struct Log {
  bytes32[] topics;
  bytes data;
  address emitter;
}

function getRecordedLogs()
external
returns (
  Log[] memory
);
```

### 描述

获取由 [`recordLogs`](./record-logs.md) 记录的已发出事件。

调用此函数时会消耗记录的日志。

### 示例

```solidity
/// event LogTopic1(
///   uint256 indexed topic1,
///   bytes data
/// );

/// event LogTopic12(
///   uint256 indexed topic1,
///   uint256 indexed topic2,
///   bytes data
/// );

/// bytes memory testData0 = "Some data";
/// bytes memory testData1 = "Other data";


// 启动记录器
vm.recordLogs();

emit LogTopic1(10, testData0);
emit LogTopic12(20, 30, testData1);

// 注意你的条目是 <Interface>.Log[]
// 而不是 <instance>.Log[]
Vm.Log[] memory entries = vm.getRecordedLogs();

assertEq(entries.length, 2);

// 记住 topics[0] 是事件签名
assertEq(entries[0].topics.length, 2);
assertEq(entries[0].topics[0], keccak256("LogTopic1(uint256,bytes)"));
assertEq(entries[0].topics[1], bytes32(uint256(10)));
// assertEq 不会比较 bytes 变量。尝试使用字符串代替。
assertEq(abi.decode(entries[0].data, (string)), string(testData0));

assertEq(entries[1].topics.length, 3);
assertEq(entries[1].topics[0], keccak256("LogTopic12(uint256,uint256,bytes)"));
assertEq(entries[1].topics[1], bytes32(uint256(20)));
assertEq(entries[1].topics[2], bytes32(uint256(30)));
assertEq(abi.decode(entries[1].data, (string)), string(testData1));

// 发出另一个事件
emit LogTopic1(40, testData0);

// 你上次的读取消耗了记录的日志，
// 你只会得到在那次调用之后最新发出的事件
entries = vm.getRecordedLogs();

assertEq(entries.length, 1);

assertEq(entries[0].topics.length, 2);
assertEq(entries[0].topics[0], keccak256("LogTopic1(uint256,bytes)"));
assertEq(entries[0].topics[1], bytes32(uint256(40)));
assertEq(abi.decode(entries[0].data, (string)), string(testData0));

```
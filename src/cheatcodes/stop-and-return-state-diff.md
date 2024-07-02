## `stopAndReturnStateDiff`

### 签名

```solidity
enum AccountAccessKind {
    Call,
    DelegateCall,
    CallCode,
    StaticCall,
    Create,
    SelfDestruct,
    Resume,
    Balance,
    Extcodesize,
    Extcodehash,
    Extcodecopy
}

struct ChainInfo {
    uint256 forkId;
    uint256 chainId;
}

struct AccountAccess {
    ChainInfo chainInfo;
    AccountAccessKind kind;
    address account;
    address accessor;
    bool initialized;
    uint256 oldBalance;
    uint256 newBalance;
    bytes deployedCode;
    uint256 value;
    bytes data;
    bool reverted;
    StorageAccess[] storageAccesses;
    uint64 depth;
}

struct StorageAccess {
    address account;
    bytes32 slot;
    bool isWrite;
    bytes32 previousValue;
    bytes32 newValue;
    bool reverted;
}

function stopAndReturnStateDiff() external returns (AccountAccess[] memory accesses);
```

### 描述

检索在调用 [`startStateDiffRecording`](./start-state-diff-recording.md) 之后记录的状态变化。此函数将在调用时消耗记录的状态差异并禁用状态差异记录。可以调用 `startStateDiffRecording` 以恢复记录。

有两种类型的状态变化记录；账户访问和存储访问，分别表示为 `AccountAccess` 和 `StorageAccess`。

账户状态变化（`AccountAccess`）在新的 EVM 上下文开始时记录；即由各种 CREATE、CALL 和 SELFDESTRUCT 操作引起。
一个 `AccountAccess` 记录包含存储访问，表示为 `StorageAccess`，这些访问在通过子调用或创建操作被抢占之前发生。

`AccountAccess` 记录的顺序反映了其相关操作的 EVM 执行顺序。每当创建或恢复 EVM 上下文时，都会创建一个 `AccountAccess`。
如果创建了子上下文，则记录一个 `Resume` `AccountAccess`，以指示之前被抢占的 `AccountAccess` 已恢复。

### `AccountAccessKind`

访问账户的类型，决定了被访问的 `account`。这通常由启动账户执行上下文的 EVM 操作指定。
如果类型是 `Call`、`DelegateCall`、`StaticCall` 或 `CallCode`，则 `account` 是被调用者。
如果类型是 Create，则账户是新创建的账户。
如果类型是 SelfDestruct，则账户是自毁接收者。
如果类型是 Resume，则账户表示已恢复的执行上下文。

- `Call` - 账户被调用
- `DelegateCall` - 账户通过委托调用被调用
- `CallCode` - 账户通过 callcode 被调用
- `StaticCall` - 账户通过 staticcall 被调用
- `Create` - 账户被创建
- `SelfDestruct` - 账户被自毁
- `Resume` - 指示之前被抢占的账户访问已恢复
- `Balance` - 账户的代码大小被读取
- `Extcodesize` - 账户的代码大小被读取
- `Extcodehash` - 账户的代码哈希被读取
- `Extcodecopy` - 账户的代码被复制

### `AccountAccess`

- `chainInfo` - 访问发生的链和分叉
- `kind` - 账户访问的类型。这决定了如何解释 `AccountAccess`
- `account` - 被访问的账户。对于 `AccountAccessKind.Create`，它是创建的账户。
  在 `AccountAccessKind.SelfDestruct` 的情况下，它是自毁接收者。
  对于所有其他类型的 `AccountAccessKind`，它是当前 EVM 上下文的账户。
- `accessor` - 访问 `account` 的对象。即账户创建者、调用者或被自毁的账户。
- `initialized` - 访问前账户是否已初始化或为空。
  如果账户有代码、非零 nonce 或非零余额，则认为已初始化。
- `oldBalance` - 被访问 `account` 之前的余额。
- `newBalance` - 被访问账户的潜在新余额。
  即，即使发生回滚，所有余额变化也会记录在此。
- `deployedCode` - 在 `AccountAccessKind.Create` 情况下部署的 `account` 的代码。对于所有其他账户访问类型，此字段为空。
- `value` - 随账户访问传递的值。
- `data` - 提供的输入数据（即 `msg.data`），在 `CREATE` 或 `CALL` 类型的访问中。
- `reverted` - 如果此访问在当前或父上下文中回滚。
- `storageAccesses` - 在账户访问未被抢占时进行的存储访问的有序列表。
- `depth` - 记录状态差异时遍历的调用深度。

### `StorageAccess`

在 `AccountAccess` 期间进行的存储访问。`StorageAccess` 不能在没有关联的 `AccountAccess` 的情况下存在。这意味着当在给定上下文开始记录状态差异时，在该上下文期间进行的存储访问不会被记录，因为该上下文（但不是其子上下文）未被记录。

`StorageAccess` 包含以下字段：

- `account` - 其存储被访问的账户
- `slot` - 被访问的槽
- `isWrite` - 访问是否为写操作
- `previousValue` - 此存储访问前的槽值
- `newValue` - 此存储访问后的槽值
- `reverted` - 如果此访问被回滚

### 恢复的 `AccountAccess`

当子上下文返回到其父上下文时，会生成这种类型的 AccountAccess。它保留与原始上下文相同的值，包括 `accessor`、`account`、`initialized`、`storageAccesses` 和 `reverted`。
以下控制流表说明了如何记录恢复的 AccountAccess。

| 合约 A 的 `alpha()` 步骤 | 合约 B 的 `beta()` 步骤 | AccountAccess 记录状态              |
|--------------------------------|-------------------------------|------------------------------------------|
| 调用 A.alpha()                |                               | [A.call]              |
| 访问状态                |                               | [A.call[A.access]]              |
| 调用 B.beta()               | B.beta() 开始               | [A.call[A.access], B.call]                  |
| (执行暂停)          | 访问状态               | [A.call[A.access], B.call[B.access]]                  |
|                             |   返回                     |                        |
| 恢复执行            | (返回到 A.alpha())         | [A.call[A.access], B.call[B.access]]             |
| 访问状态                 |                              | [<br>&emsp;A.call[A.access], <br>&emsp;B.call[B.access], <br>&emsp;A.resume[A.access']<br>]       |


> ℹ️ **注意**
>
> 仅当上下文恢复后发生存储访问时，才会创建恢复的 AccountAccess。

### 示例：在 CREATE 操作期间记录存储状态变化
```solidity
contract Contract {
    uint256 internal _reserved;
    uint256 public data;
    constructor(uint _data) payable { data = _data; }
}

vm.startStateDiffRecording();
Contract contract = new Contract{value: 1 ether}(100);
Vm.AccountAccess[] memory records = vm.stopAndReturnStateDiff();

assertEq(records.length, 1);
assertEq(records[0].kind, Vm.AccountAccessKind.Create);
assertEq(records[0].account, address(contract));
assertEq(records[0].accessor, address(this));
assertEq(records[0].initialized, true);
assertEq(records[0].oldBalance, 0);
assertEq(records[0].newBalance, 1 ether);
assertEq(records[0].deployedCode, address(contract).code);
assertEq(records[0].value, 1 ether);
assertEq(records[0].data, abi.encodePacked(type(Contract).creationCode, (uint(100))));
assertEq(records[0].reverted, false);

assertEq(records[0].storageAccesses.length, 1);
assertEq(records[0].storageAccesses[0].account, address(contract));
assertEq(records[0].storageAccesses[0].slot, bytes32(uint256(1)));
assertEq(records[0].storageAccesses[0].isWrite, true);
assertEq(records[0].storageAccesses[0].previousValue, bytes32(uint(0)));
assertEq(records[0].storageAccesses[0].newValue, bytes32(uint(100)));
assertEq(records[0].storageAccesses[0].reverted, false);
```

注意，此示例中没有 `Resume` 账户访问。

### 示例：恢复的账户访问
```solidity
contract Foo {
    Bar b;
    uint256 public val;
    constructor(Bar _b) { b = _b; }
    function run() external {
        val = val + 1;
        b.run();
        val = val + 1;
    }
}
contract Bar {
    function run() external {}
}

Bar bar = new Bar();
Foo foo = new Foo(bar);

vm.startStateDiffRecording();
foo.run();
Vm.AccountAccess[] memory records = vm.stopAndReturnStateDiff();

assertEq(records.length, 3);
Vm.AccountAccess memory fooCall = records[0];
assertEq(fooCall.kind, Vm.AccountAccessKind.Call);
assertEq(fooCall.account, address(foo));
assertEq(fooCall.accessor, address(this));
// foo.val 增加
assertEq(fooCall.storageAccesses.length, 2);
assertEq(fooCall.storageAccesses[0].isWrite, false);
assertEq(fooCall.storageAccesses[1].isWrite, true);
assertEq(fooCall.storageAccesses[1].oldValue, bytes32(uint(0)));
assertEq(fooCall.storageAccesses[1].newValue, bytes32(uint(1)));

// bar.run CALL
Vm.AccountAccess memory barCall = records[1];
assertEq(barCall.kind, Vm.AccountAccessKind.Call);
assertEq(barCall.account, address(bar));
assertEq(barCall.accessor, address(foo));

// foo.run RESUME
Vm.AccountAccess memory fooResume = records[2];
assertEq(fooResume.kind, Vm.AccountAccessKind.Resume);
// foo.val 增加
assertEq(fooResume.storageAccesses.length, 2);
assertEq(fooResume.storageAccesses[0].isWrite, false);
assertEq(fooResume.storageAccesses[1].isWrite, true);
assertEq(fooResume.storageAccesses[1].oldValue, bytes32(uint(1)));
assertEq(fooResume.storageAccesses[1].newValue, bytes32(uint(2)));
```
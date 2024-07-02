## 快照作弊码

### 签名

```solidity
// 快照当前的 EVM 状态。
// 返回创建的快照的 ID。
// 要恢复快照，请使用 `revertTo`
function snapshot() external returns(uint256);
// 将 EVM 状态恢复到之前的快照
// 接受要恢复的快照 ID。
// 这将删除该快照以及在该快照之后创建的所有快照。
function revertTo(uint256) external returns(bool);
```

### 描述

`snapshot` 对区块链的状态进行快照，并返回创建的快照的标识符。

`revertTo` 将区块链的状态恢复到给定的快照。这将删除给定的快照，以及在该快照之后创建的所有快照（例如：恢复到 ID 为 2 的快照将删除 ID 为 2、3、4 等的快照）。

### 示例

```solidity
struct Storage {
    uint slot0;
    uint slot1;
}

contract SnapshotTest is Test {
    Storage store;
    uint256 timestamp;

    function setUp() public {
        store.slot0 = 10;
        store.slot1 = 20;
        vm.deal(address(this), 5 ether);        // 余额 = 5 ether
        timestamp = block.timestamp;
    }

    function testSnapshot() public {
        uint256 snapshot = vm.snapshot();       // 保存状态

        // 让我们改变状态
        store.slot0 = 300;
        store.slot1 = 400;
        vm.deal(address(this), 500 ether);
        vm.warp(12345);                         // block.timestamp = 12345

        assertEq(store.slot0, 300);
        assertEq(store.slot1, 400);
        assertEq(address(this).balance, 500 ether);
        assertEq(block.timestamp, 12345);

        vm.revertTo(snapshot);                  // 恢复状态

        assertEq(store.slot0, 10, "slot 0 的快照恢复不成功");
        assertEq(store.slot1, 20, "slot 1 的快照恢复不成功");
        assertEq(address(this).balance, 5 ether, "余额的快照恢复不成功");
        assertEq(block.timestamp, timestamp, "时间戳的快照恢复不成功");
    }
}
```
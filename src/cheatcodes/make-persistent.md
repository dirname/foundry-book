## `makePersistent`

### 签名

```solidity
function makePersistent(address account) external;
function makePersistent(address account0, address account1) external;
function makePersistent(address account0, address account1, address account2) external;
function makePersistent(address[] calldata accounts) external;
```

### 描述

每个分叉（[`createFork`](./create-fork.md)）都有其独立的存储空间，当选择另一个分叉时（[`selectFork`](./select-fork.md)），存储空间也会被替换。
默认情况下，只有测试合约账户和调用者是跨分叉持久的，这意味着当选择不同的分叉时，测试合约的状态（变量）变化会被保留。这样可以通过将数据存储在合约的变量中来共享数据。

然而，通过这个作弊码，可以将指定的账户标记为持久，这意味着无论当前激活的是哪个分叉，它们的状态都是可用的。

### 示例

将新合约标记为持久

```solidity
contract SimpleStorageContract {
    string public value;

    function set(uint256 _value) public {
        value = _value;
    }
}

function testMarkPersistent() public {
    // 默认情况下，`sender` 和合约本身是持久的
    assert(cheats.isPersistent(msg.sender));
    assert(cheats.isPersistent(address(this)));

    // 选择一个特定的分叉
    cheats.selectFork(mainnetFork);
    
    // 创建一个存储在 `mainnetFork` 存储空间中的新合约
    SimpleStorageContract simple = new SimpleStorageContract();
    
    // `simple` 未被标记为持久
    assert(!cheats.isPersistent(address(simple)));
    
    // 合约可以使用
    uint256 expectedValue = 99;
    simple.set(expectedValue);
    assertEq(simple.value(), expectedValue);
    
    // 标记为持久
    cheats.makePersistent(address(simple));
    
    // 选择一个不同的分叉
    cheats.selectFork(optimismFork);
    
    // 确保合约仍然是持久的   
    assert(cheats.isPersistent(address(simple)));
    
    // 值设置如预期
    assertEq(simple.value(), expectedValue);
}
```

### 参见

- [isPersistent](./is-persistent.md)
- [revokePersistent](./revoke-persistent.md)
- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
## `revokePersistent`

### 签名

```solidity
function revokePersistent(address) external;
function revokePersistent(address[] calldata) external;
```

### 描述

这是 [`makePersistent`](./make-persistent.md) 的对应函数，使给定的合约在分叉交换中不再持久。

### 示例

撤销合约的持久状态

```solidity
contract SimpleStorageContract {
    string public value;

    function set(uint256 _value) public {
        value = _value;
    }
}

function testRevokePersistent() public {
    // 选择一个特定的分叉
    cheats.selectFork(mainnetFork);
    
    // 在 `mainnetFork` 存储中创建一个新的合约
    SimpleStorageContract simple = new SimpleStorageContract();
    
    // `simple` 未被标记为持久
    assert(!cheats.isPersistent(address(simple)));
       
    // 使其持久
    cheats.makePersistent(address(simple));
    
    // 确保它是持久的
    assert(cheats.isPersistent(address(simple)));
    
    // 撤销持久状态
    cheats.revokePersistent(address(simple));
    
    // 合约不再持久
    assert(!cheats.isPersistent(address(simple)));
}
```

### 参见

- [isPersistent](./is-persistent.md)
- [revokePersistent](./revoke-persistent.md)
- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
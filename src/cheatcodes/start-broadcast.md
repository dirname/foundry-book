## `startBroadcast`

### 签名

```solidity
function startBroadcast() external;
```

```solidity
function startBroadcast(address who) external;
```

```solidity
function startBroadcast(uint256 privateKey) external;
```

### 描述

使用调用测试合约的地址或提供的地址/私钥作为发送者，所有后续调用（仅在此调用深度且不包括作弊码调用）将创建可以稍后签名并发送上链的交易。

### 示例

```solidity
function t(uint256 a) public returns (uint256) {
    uint256 b = 0;
    emit log_string("here");
    return b;
}

function deployOther() public {
    vm.startBroadcast(ACCOUNT_A);
    Test test = new Test();
    
    // 将触发一笔交易
    test.t(1);
    
    vm.stopBroadcast();

    // 再次广播，这次使用环境变量中的私钥
    vm.startBroadcast(vm.envUint("PRIVATE_KEY"));
    test.t(3);
    vm.stopBroadcast();
}
```

### 另请参阅

- [broadcast](./broadcast.md)
- [stopBroadcast](./stop-broadcast.md)
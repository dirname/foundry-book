## `broadcast`

### 签名

```solidity
function broadcast() external;
```

```solidity
function broadcast(address who) external;
```

```solidity
function broadcast(uint256 privateKey) external;
```

### 描述

使用调用测试合约的地址或提供的地址/私钥作为发送者，使得下一个调用（仅在此调用深度且不包括作弊码调用）创建一个可以稍后签名并发送到链上的交易。

### 示例

```solidity
function deploy() public {
    vm.broadcast(ACCOUNT_A);
    Test test = new Test();

    // 这不会生成需要签名的交易
    uint256 b = test.t(4);

    // 这会生成需要签名的交易
    vm.broadcast(ACCOUNT_B);
    test.t(2);

    // 这也会生成需要签名的交易，使用环境变量中的私钥
    vm.broadcast(vm.envUint("PRIVATE_KEY"));
    test.t(3);
} 
```

### 参见

- [startBroadcast](./start-broadcast.md)
- [stopBroadcast](./stop-broadcast.md)
## `transact`

### 签名

```solidity
// 从当前激活的分叉中获取给定的交易并执行它
function transact(bytes32 txHash) external;
// 从指定的分叉中获取给定的交易并执行它
function transact(uint256 forkId, bytes32 txHash) external;
```

### 描述

在分叉模式下，从提供者获取交易并在当前状态下执行它。

### 示例

进入分叉模式并执行一笔交易：

```solidity
// 在区块 https://etherscan.io/block/15596646 进入分叉模式
uint256 fork = vm.createFork(MAINNET_RPC_URL, 15596646);
vm.selectFork(fork);

// 区块中的随机转账交易：https://etherscan.io/tx/0xaba74f25a17cf0d95d1c6d0085d6c83fb8c5e773ffd2573b99a953256f989c89
bytes32 tx = 0xaba74f25a17cf0d95d1c6d0085d6c83fb8c5e773ffd2573b99a953256f989c89;

address sender = address(0xa98218cdc4f63aCe91ddDdd24F7A580FD383865b);
address recipient = address(0x0C124046Fa7202f98E4e251B50488e34416Fc306);

assertEq(sender.balance, 5764124000000000);
assertEq(recipient.balance, 3936000000000000);

// 转账金额：0.003936 Ether
uint256 transferAmount = 3936000000000000;

// 执行交易后的预期余额变化
uint256 expectedRecipientBalance = recipient.balance + transferAmount;
uint256 expectedSenderBalance = sender.balance - transferAmount;

// 执行交易
vm.transact(tx);

// 接收者收到转账
assertEq(recipient.balance, expectedRecipientBalance);

// 发送者的余额因转账和 gas 费用减少
assert(sender.balance < expectedSenderBalance);
```

### 参见

- [roll](./roll.md)
- [createFork](./create-fork.md)
- [selectFork](./select-fork.md)
- [activeFork](./active-fork.md)
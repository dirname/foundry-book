## `expectEmit`

### 签名

```solidity
function expectEmit() external;
```

```solidity
function expectEmit(
    bool checkTopic1,
    bool checkTopic2,
    bool checkTopic3,
    bool checkData
) external;
```

```solidity
function expectEmit(address emitter) external;
```

```solidity
function expectEmit(
    bool checkTopic1,
    bool checkTopic2,
    bool checkTopic3,
    bool checkData,
    address emitter
) external;
```

### 描述

断言在下次调用期间会发出特定的日志。

1. 调用作弊代码，指定我们是否应该检查第一个、第二个或第三个主题，以及日志数据（`expectEmit()` 检查所有这些）。主题 0 总是被检查。
2. 在下次调用期间发出我们预期会看到的事件。
3. 执行调用。

你可以在下次调用中多次执行步骤 1 和 2 以匹配一系列事件。

如果事件在当前作用域中不可用（例如，如果我们使用接口或外部智能合约），我们可以用相同的事件签名自己定义事件。

`expectEmit` 有两种变体：

- **不检查发射地址**：断言主题匹配**不**检查发射地址。
- **带 `address`**：断言主题匹配并且发射地址匹配。

> ℹ️ **匹配序列**
>
> 在发出大量事件的函数中，可以“跳过”事件并仅匹配特定序列，但此序列必须始终按顺序排列。例如，假设一个函数发出事件：`A, B, C, D, E, F, F, G`。
>
> `expectEmit` 将能够匹配范围，并且可以在中间跳过事件：
> - `[A, B, C]` 是有效的。
> - `[B, D, F]` 是有效的。
> - `[G]` 或其他单一事件组合是有效的。
> - `[B, A]` 或类似的乱序组合是**无效的**（事件必须按顺序排列）。
> - `[C, F, F]` 是有效的。
> - `[F, F, C]` 是**无效的**（乱序）。

### 示例

这不检查发射地址。

```solidity
event Transfer(address indexed from, address indexed to, uint256 amount);

function testERC20EmitsTransfer() public {
    vm.expectEmit();

    // 我们发出预期会看到的事件。
    emit MyToken.Transfer(address(this), address(1), 10);

    // 我们执行调用。
    myToken.transfer(address(1), 10);
}
```

这检查发射地址。

```solidity
event Transfer(address indexed from, address indexed to, uint256 amount);

function testERC20EmitsTransfer() public {
    // 我们通过传递地址来检查代币是否是事件发射者。
    vm.expectEmit(address(myToken));
    emit MyToken.Transfer(address(this), address(1), 10);

    // 我们执行调用。
    myToken.transfer(address(1), 10);
}
```

我们还可以断言在一次调用中发出多个事件。

```solidity
function testERC20EmitsBatchTransfer() public {
    // 我们声明多个预期的转账事件
    for (uint256 i = 0; i < users.length; i++) {
        // 这里我们使用较长的签名进行演示。此调用检查
        // topic0（总是检查），topic1（true），topic2（true），不检查 topic3（false），以及数据（true）。
        vm.expectEmit(true, true, false, true);
        emit Transfer(address(this), users[i], 10);
    }

    // 我们还预期一个自定义的 `BatchTransfer(uint256 numberOfTransfers)` 事件。
    vm.expectEmit(false, false, false, true);
    emit BatchTransfer(users.length);

    // 我们执行调用。
    myToken.batchTransfer(users, 10);
}
```

此示例失败，因为预期的事件在下次调用中未发出。
```solidity
event Transfer(address indexed from, address indexed to, uint256 amount);

function testERC20EmitsTransfer() public {
    // 我们通过传递地址作为第五个参数来检查代币是否是事件发射者。
    vm.expectEmit(true, true, false, true, address(myToken));
    emit MyToken.Transfer(address(this), address(1), 10);

    // 我们执行一个不相关的调用，该调用不会发出预期的事件，
    // 使作弊代码失败。
    myToken.approve(address(this), 1e18);
    // 我们执行调用，但它不会有任何效果，因为作弊代码已经失败。
    myToken.transfer(address(1), 10);
}
```
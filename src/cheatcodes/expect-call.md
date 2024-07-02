## `expectCall`

```solidity
function expectCall(address callee, bytes calldata data) external;
```

```solidity
function expectCall(address callee, bytes calldata data, uint64 count) external;
```

```solidity
function expectCall(
    address callee,
    uint256 value,
    bytes calldata data
) external;
```

```solidity
function expectCall(
    address callee,
    uint256 value,
    bytes calldata data,
    uint64 count
) external;
```

### 描述

期望对指定地址 `callee` 进行调用，其中调用数据要么严格匹配 `data`，要么宽松匹配 `data`。该作弊码有两种调用方式：

- 如果没有指定 `count` 参数，则期望调用至少被调用的次数与作弊码被调用的次数相同。对于相同的调用数据，不能在没有 `count` 的情况下调用作弊码，然后传入 `count` 参数。
- 如果指定了 `count`，则期望调用严格被调用 `count` 次。对于相同的调用数据，`count` 值不能通过另一个作弊码调用被覆盖，也不能通过不带 `count` 参数的作弊码调用增加。

`count` 也可以设置为 0，以断言不会进行调用。

当对 `callee` 进行调用时，首先检查调用数据是否与 `data` 完全匹配。如果没有，则检查调用数据是否存在部分匹配，匹配从调用数据的第一个字节开始。

**使用第二个签名**，我们还可以检查调用是否使用预期的 `msg.value` 进行。

如果测试在没有进行调用的情况下终止，则测试失败。

> ℹ️ **内部调用**
>
> 该作弊码目前不适用于内部调用。请参见问题 [#432](https://github.com/foundry-rs/foundry/issues/432)。

### 示例

期望在代币 `MyToken` 上调用 `transfer` 一次：

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
vm.expectCall(
  address(token), abi.encodeCall(token.transfer, (alice, 10))
);
token.transfer(alice, 10);
// [PASS]
```

期望在代币 `MyToken` 上调用 `transfer` *至少* 两次：

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
vm.expectCall(
  address(token), abi.encodeCall(token.transfer, (alice, 10))
);
vm.expectCall(
  address(token), abi.encodeCall(token.transfer, (alice, 10))
);
token.transfer(alice, 10);
token.transfer(alice, 10);
token.transfer(alice, 10);
// [PASS]
```

期望在代币 `MyToken` 上不调用 `transfer`：

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
vm.expectCall(
  address(token), abi.encodeCall(token.transfer, (alice, 10)), 0
);
token.transferFrom(alice, address(0), 10);
// [PASS]
```

期望在代币 `MyToken` 上调用 `transfer` 且任何调用数据被调用 2 次：

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
vm.expectCall(
  address(token), abi.encodeWithSelector(token.transfer.selector), 2
);
token.transfer(alice, 10);
token.transfer(alice, 10);
// [PASS]
```

期望在 `Contract` 上调用 `pay` 且具有特定的 `msg.value` 和 `calldata`：

```solidity
Contract target = new Contract();
vm.expectCall(
            address(target),
            1,
            abi.encodeWithSelector(target.pay.selector, 2)
        );
target.pay{value: 1}(2);
// [PASS]
```

期望在 `Contract` 上调用 `pay` 且具有特定的 `msg.value` 和 `calldata` 3 次：

```solidity
Contract target = new Contract();
vm.expectCall(
            address(target),
            1,
            abi.encodeWithSelector(target.pay.selector, 2),
            3
        );
target.pay{value: 1}(2);
target.pay{value: 1}(2);
target.pay{value: 1}(2);
// [PASS]
```
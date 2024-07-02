## `expectRevert`

### 签名

```solidity
function expectRevert() external;
```

```solidity
function expectRevert(bytes4 message) external;
```

```solidity
function expectRevert(bytes calldata message) external;
```

### 描述

如果**下一次调用**没有以预期的数据 `message` 回滚，那么 `expectRevert` 将会回滚。

在调用 `expectRevert` 之后，在回滚调用之前的其他作弊码调用将被忽略。

这意味着，例如，我们可以在回滚调用之前立即调用 [`prank`](./prank.md)。

有三种签名：

- **无参数**：断言下一次调用会回滚，无论消息是什么。
- **带 `bytes4`**：断言下一次调用会以指定的 4 字节回滚。
- **带 `bytes`**：断言下一次调用会以指定的字节回滚。

> ⚠️ **注意：与低级调用一起使用**
>
> 通常，成功的调用会返回 `true` 的状态（以及任何返回数据），而回滚的调用会返回 `false`。
>
> Solidity 编译器会插入检查以确保调用成功，并在失败时回滚。
>
> 在低级调用中，`expectRevert` 作弊码通过使低级调用返回的 `status` 布尔值对应于 `expectRevert` 是否成功，而不是低级调用是否成功。因此，`status` 为 false 对应于作弊码失败。
>
> 除此之外，`expectRevert` 还会在低级调用中篡改返回数据，并且不可用。
>
> 请参见以下示例。为了清晰起见，`status` 已重命名为 `revertsAsExpected`：
>
> ```solidity
> function testLowLevelCallRevert() public {
>     vm.expectRevert(bytes("error message"));
>     (bool revertsAsExpected, ) = address(myContract).call(myCalldata);
>     assertTrue(revertsAsExpected, "expectRevert: call did not revert");
> }
> ```

### 示例

要使用 `expectRevert` 与 `string`，将其作为字符串字面量传递。

```solidity
vm.expectRevert("error message");
```

要使用 `expectRevert` 与不带参数的自定义 [错误类型][error-type]，使用其选择器。

```solidity
vm.expectRevert(CustomError.selector);
```

要使用 `expectRevert` 与带参数的自定义 [错误类型][error-type]，使用 ABI 编码错误类型。

```solidity
vm.expectRevert(
    abi.encodeWithSelector(CustomError.selector, 1, 2)
);
```

如果你需要断言一个函数回滚**没有**消息，可以使用 `expectRevert(bytes(""))`。

```solidity
function testExpectRevertNoReason() public {
    Reverter reverter = new Reverter();
    vm.expectRevert(bytes(""));
    reverter.revertWithoutReason();
}
```

无消息的回滚发生在出现 EVM 错误时，例如当交易消耗的 gas 超过区块的 gas 限制时。

如果你需要断言一个函数回滚一个四字符的消息，例如 `AAAA`，可以使用：

```solidity
function testFourLetterMessage() public {
    vm.expectRevert(bytes("AAAA"));
}
```

如果使用 `expectRevert("AAAA")`，编译器会抛出错误，因为它不知道使用哪个重载。

最后，你也可以在一个测试中有多个 `expectRevert()` 检查。

```solidity
function testMultipleExpectReverts() public {
    vm.expectRevert("INVALID_AMOUNT");
    vault.send(user, 0);

    vm.expectRevert("INVALID_ADDRESS");
    vault.send(address(0), 200);
}
```

### 另请参阅

Forge 标准库

[Std Errors](../reference/forge-std/std-errors.md)

[error-type]: https://docs.soliditylang.org/en/v0.8.11/contracts.html#errors
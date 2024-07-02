## `mockCallRevert`

### 签名

```solidity
function mockCallRevert(address where, bytes calldata data, bytes calldata retdata) external;
```

```solidity
function mockCallRevert(
    address where,
    uint256 value,
    bytes calldata data,
    bytes calldata retdata
) external;
```

### 描述

如果调用数据严格或宽松匹配 `data`，则对地址 `where` 的所有调用都会被还原，并返回 `retdata`。

`retdata` 可以是原始返回消息或自定义错误。

当对 `where` 进行调用时，首先检查调用数据是否与 `data` 完全匹配。
如果不匹配，则检查调用数据是否存在部分匹配，匹配从调用数据的第一个字节开始。

如果找到匹配，则调用被还原，并返回 `retdata`。

**使用第二个签名**，我们可以使用特定的 `msg.value` 模拟调用。如果存在歧义，调用数据匹配优先于 `msg.value`。

被还原的模拟调用一直有效，直到调用 [`clearMockedCalls`](./clear-mocked-calls.md)。

> ℹ️ **内部调用**
>
> 此作弊代码目前不适用于内部调用。请参见问题 [#432](https://github.com/foundry-rs/foundry/issues/432)。

### 示例

使用原始错误消息还原精确调用：

```solidity
function testMockCallRevert() public {
    vm.mockCallRevert(
        address(0),
        abi.encodeWithSelector(MyToken.balanceOf.selector, address(1)),
        "REVERT_MESSAGE"
    );
    vm.expectRevert("REVERT_MESSAGE");
    IERC20(address(0)).balanceOf(address(1));
}
```

使用自定义错误还原调用：

```solidity
function testMockCallRevertWithCustomError() public {
    bytes memory customError = abi.encodeWithSelector(TestError.selector, "ERROR_MESSAGE");
    vm.mockCallRevert(
        address(0),
        abi.encodeWithSelector(MyToken.balanceOf.selector, address(1)),
        customError
    );
    vm.expectRevert(customError);
    IERC20(address(0)).balanceOf(address(1));
}
```

使用给定的 `msg.value` 模拟调用：

```solidity
function testMockCallRevertWithValue() public {
    assertEq(example.pay{value: 10}(1), 1);
    assertEq(example.pay{value: 1}(2), 2);
    vm.mockCallRevert(
        address(example),
        10,
        abi.encodeWithSelector(example.pay.selector),
        "ERROR_MESSAGE"
    );
    assertEq(example.pay{value: 1}(2), 2);
    vm.expectRevert("ERROR_MESSAGE");
    assertEq(example.pay{value: 10}(1), 99);
}
```
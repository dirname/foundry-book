## `skip`

## 签名

```solidity
function skip(bool skip) external;
```

## 描述

将测试标记为跳过，条件性地。必须在测试的顶部调用它，以确保在没有任何执行的情况下跳过测试。

如果 `skip` 被调用时传入一个 false 布尔值，它将不会跳过测试。

标记为跳过的测试将在测试运行器和摘要中显示 `[SKIPPED]` 标签，以便轻松识别跳过的测试。

### 示例

```solidity

function testSkip() public {
    vm.skip(true);
    /// 这个 revert 不会被触发，因为这个测试将被跳过。
    revert("Should not reach this revert");
}

function testNotSkip() public {
    vm.skip(false);
    /// 这个 revert 将被触发，因为这个测试不会被跳过，并且测试将失败。
    revert("Should reach this revert");
}
```
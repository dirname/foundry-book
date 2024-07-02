## `assume`

### 签名

```solidity
function assume(bool) external;
```

### 描述

如果布尔表达式评估为 false，模糊测试器将丢弃当前的模糊输入并开始新的模糊测试运行。

`assume` 作弊码主要应用于非常窄的检查。
广泛的检查会减慢测试速度，因为需要一段时间才能找到有效的值，并且如果达到最大拒绝次数，测试可能会失败。

您可以通过在 `foundry.toml` 文件中设置 [`fuzz.max_test_rejects`][max-test-rejects] 来配置拒绝阈值。

对于广泛的检查，例如确保 `uint256` 落在某个范围内，您可以使用取模运算符或 Forge Standard 的 [`bound`][forge-std-bound] 方法来限制输入。

有关通过 `assume` 进行过滤的更多信息，请参见[这里][filtering-guide]。

### 示例

```solidity
// 使用 assume 的好例子
function testSomething(uint256 a) public {
    vm.assume(a != 1);
    require(a != 1);
    // [PASS]
}
```

```solidity
// 在这种情况下，assume 不是很好的选择，因此您应该手动限制输入
function testSomethingElse(uint256 a) public {
    a = bound(a, 100, 1e36);
    require(a >= 100 && a <= 1e36);
    // [PASS]
}
```

### 另请参阅

Forge Standard Library

[`bound`](../reference/forge-std/bound.md)

[max-test-rejects]: ../reference/config/testing.md#max_test_rejects
[forge-std-bound]: ../reference/forge-std/bound.md
[filtering-guide]: https://altsysrq.github.io/proptest-book/proptest/tutorial/filtering.html#filtering
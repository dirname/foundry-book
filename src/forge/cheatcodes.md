## 作弊码

大多数情况下，仅仅测试智能合约的输出是不够的。为了操纵区块链的状态，以及测试特定的回滚和事件，Foundry 提供了一组作弊码。

作弊码允许你改变区块号、身份等。它们通过在特定地址上调用特定函数来调用：`0x7109709ECfa91a80626fF3989D68f67F5b1DD12D`。

你可以通过 Forge 标准库中的 `Test` 合约中可用的 `vm` 实例轻松访问作弊码。Forge 标准库在后面的[章节](./forge-std.md)中有更详细的解释。

让我们为一个只能由其所有者调用的智能合约编写测试。

```solidity
{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:prelude}}

{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:contract}}

{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:contract_prelude}}

{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:simple_test}}
}
```

如果我们现在运行 `forge test`，我们会看到测试通过，因为 `OwnerUpOnlyTest` 是 `OwnerUpOnly` 的所有者。

```ignore
$ forge test
{{#include ../output/cheatcodes/forge-test-simple:output}}
```

让我们确保一个绝对不是所有者的人不能增加计数：

```solidity
{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:contract_prelude}}

    // ...

{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:test_fail}}
}
```

如果我们现在运行 `forge test`，我们会看到所有测试都通过了。

```ignore
$ forge test
{{#include ../output/cheatcodes/forge-test-cheatcodes:output}}
```

测试通过是因为 `prank` 作弊码将我们的身份更改为零地址，以便下一次调用（`upOnly.increment()`）。测试用例通过是因为我们使用了 `testFail` 前缀，然而，使用 `testFail` 被认为是一种反模式，因为它没有告诉我们 `upOnly.increment()` 回滚的任何原因。

如果我们再次运行带有跟踪的测试，我们可以看到我们回滚了正确的错误消息。

```ignore
$ forge test -vvvv --match-test testFail_IncrementAsNotOwner
{{#include ../output/cheatcodes/forge-test-cheatcodes-tracing:output}}
```

为了确保将来我们回滚是因为我们不是所有者，让我们使用 `expectRevert` 作弊码：

```solidity
{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:contract_prelude}}

    // ...

{{#include ../../projects/cheatcodes/test/OwnerUpOnly.t.sol:test_expectrevert}}
}
```

如果我们最后一次运行 `forge test`，我们看到测试仍然通过，但这次我们确定如果因为任何其他原因回滚，它将始终失败。

```ignore
$ forge test
{{#include ../output/cheatcodes/forge-test-cheatcodes-expectrevert:output}}
```

另一个可能不太直观的作弊码是 `expectEmit` 函数。在查看 `expectEmit` 之前，我们需要了解什么是事件。

事件是合约的可继承成员。当你发出一个事件时，参数会存储在区块链上。最多可以将事件的三个参数添加 `indexed` 属性，以形成称为“主题”的数据结构。主题允许用户在区块链上搜索事件。

```solidity
{{#include ../../projects/cheatcodes/test/EmitContract.t.sol:all}}
```

当我们调用 `vm.expectEmit(true, true, false, true);` 时，我们希望检查下一个事件的第 1 和第 2 个 `indexed` 主题。

`test_ExpectEmit()` 中的预期 `Transfer` 事件意味着我们期望 `from` 是 `address(this)`，而 `to` 是 `address(1337)`。这与从 `emitter.t()` 发出的事件进行比较。

换句话说，我们正在检查 `emitter.t()` 的第一个主题是否等于 `address(this)`。`expectEmit` 的第三个参数设置为 `false`，因为不需要检查 `Transfer` 事件的第三个主题，因为只有两个。即使我们设置为 `true`，也没有关系。

`expectEmit` 的第四个参数设置为 `true`，这意味着我们希望检查“非索引主题”，也称为数据。

例如，我们希望 `test_ExpectEmit` 中预期事件的数据（即 `amount`）等于实际发出事件中的数据。换句话说，我们断言 `emitter.t()` 发出的 `amount` 等于 `1337`。如果 `expectEmit` 的第四个参数设置为 `false`，我们将不会检查 `amount`。

换句话说，即使数量不同，`test_ExpectEmit_DoNotCheckData` 也是一个有效的测试用例，因为我们不检查数据。

<br>

> 📚 **参考**
>
> 请参阅[作弊码参考](../cheatcodes/)以获取所有可用作弊码的完整概述。
## 测试

Forge 可以使用 [`forge test`](../reference/forge/forge-test.md) 命令运行您的测试。所有测试都是用 Solidity 编写的。

Forge 会在您的源代码目录中查找测试。任何以 `test` 开头的函数都被视为测试。通常，测试会按照惯例放在 `test/` 目录中，并以 `.t.sol` 结尾。

以下是一个在新创建的项目中运行 `forge test` 的示例，该项目只有默认测试：

```sh
{{#include ../output/hello_foundry/forge-test:all}}
```

您还可以通过传递过滤器来运行特定的测试：

```sh
{{#include ../output/test_filters/forge-test-match-contract-and-test:all}}
```

这将运行名为 `ComplicatedContractTest` 测试合约中的 `testDeposit` 测试。这些标志的反向版本也存在（`--no-match-contract` 和 `--no-match-test`）。

您可以使用 `--match-path` 运行与 glob 模式匹配的文件名中的测试。

```sh
{{#include ../output/test_filters/forge-test-match-path:all}}
```

`--match-path` 标志的反向版本是 `--no-match-path`。

### 日志和跟踪

`forge test` 的默认行为是仅显示通过和失败测试的摘要。您可以通过增加详细程度（使用 `-v` 标志）来控制此行为。每个详细程度级别都会添加更多信息：

- **级别 2 (`-vv`)**：还会显示测试期间发出的日志。这包括测试中的断言错误，显示预期值与实际值的信息。
- **级别 3 (`-vvv`)**：还会显示失败测试的堆栈跟踪。
- **级别 4 (`-vvvv`)**：显示所有测试的堆栈跟踪，并显示失败测试的设置跟踪。
- **级别 5 (`-vvvvv`)**：始终显示堆栈跟踪和设置跟踪。

### 监视模式

Forge 可以在您对文件进行更改时重新运行测试，使用 `forge test --watch`。

默认情况下，只有更改的测试文件会重新运行。如果您希望在更改时重新运行所有测试，可以使用 `forge test --watch --run-all`。
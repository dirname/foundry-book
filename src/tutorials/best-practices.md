# 最佳实践

本指南记录了在使用 Foundry 进行开发时的建议最佳实践。通常情况下，建议尽可能使用 [`forge fmt`](../reference/config/formatter.md) 处理，任何此工具未处理的内容如下所述。

- [最佳实践](#best-practices)
  - [通用合约指南](#general-contract-guidance)
  - [测试](#tests)
    - [通用测试指南](#general-test-guidance)
    - [分叉测试](#fork-tests)
    - [测试工具](#test-harnesses)
      - [内部函数](#internal-functions)
      - [私有函数](#private-functions)
      - [工作区函数](#workaround-functions)
    - [最佳实践](#best-practices-1)
    - [污染分析](#taint-analysis)
  - [脚本](#scripts)
    - [私钥管理](#private-key-management)
  - [注释](#comments)
  - [资源](#resources)

## 通用合约指南

1. 始终使用命名导入语法，不要导入整个文件。这限制了导入的内容仅为命名的项目，而不是文件中的所有内容。导入整个文件可能会导致 solc 抱怨重复定义和 slither 出错，尤其是在仓库增长并且有更多依赖项且名称重叠时。

   - 好：`import {MyContract} from "src/MyContract.sol"` 仅导入 `MyContract`。
   - 坏：`import "src/MyContract.sol"` 导入 `MyContract.sol` 中的所有内容。（导入 `forge-std/Test` 或 `Script` 可以是一个例外，这样你可以获得控制台库等）。

1. 注意导入时绝对路径和相对路径之间的权衡（绝对路径相对于仓库根目录，例如 `"src/interfaces/IERC20.sol"`），并选择最适合你项目的方法：

   - 绝对路径更容易看到文件的来源，并且在移动文件时减少变动。
   - 相对路径使你的编辑器更有可能提供诸如 linting 和自动完成等功能，因为编辑器/扩展可能不理解你的重映射。

1. 如果从依赖项复制库（而不是导入它），请在配置文件中使用 `ignore = []` 选项以避免格式化该文件。这使得与原始文件的差异对比更简单，便于审查和审计。

1. 同样，可以自由使用 `// forgefmt: disable-*` 注释指令来忽略看起来更适合手动格式化的代码行/部分。`*` 的支持值有：

   - `disable-line`
   - `disable-next-line`
   - `disable-next-item`
   - `disable-start`
   - `disable-end`

来自 [samsczun](https://twitter.com/samczsun) 的 [How Do You Even Write Secure Code Anyways](https://www.youtube.com/watch?v=Wm3t8Fuiy1E) 演讲的额外最佳实践：

- 使用描述性变量名。
- 限制活动变量的数量。
- 没有冗余逻辑。
- 尽可能早退出以减少查看代码时的精神负担。
- 相关代码应放在一起。
- 删除未使用的代码。

## 测试

### 通用测试指南

1. 对于测试 `MyContract.sol`，测试文件应为 `MyContract.t.sol`。对于测试 `MyScript.s.sol`，测试文件应为 `MyScript.t.sol`。

   - 如果合约很大并且你想将其拆分到多个文件中，可以按逻辑分组，如 `MyContract.owner.t.sol`、`MyContract.deposits.t.sol` 等。

1. 切勿在 `setUp` 函数中进行断言，而是使用专门的测试，如 `test_SetUpState()`，如果你需要确保 `setUp` 函数按预期工作。更多信息请参见 [foundry-rs/foundry#1291](https://github.com/foundry-rs/foundry/issues/1291)。

1. 对于单元测试，有两种主要的组织测试的方法：

   1. 将合约视为描述块：

      - `contract Add` 包含 `MyContract` 的 `add` 方法的所有单元测试。
      - `contract Supply` 包含 `supply` 方法的所有测试。
      - `contract Constructor` 包含构造函数的所有测试。
      - 这种方法的好处是较小的合约应该比大合约编译得更快，因此这种方法在测试套件变大时应该节省时间。

   2. 每个被测试的合约有一个测试合约，并尽可能多地使用工具和固定装置：
      - `contract MyContractTest` 包含 `MyContract` 的所有单元测试。
      - `function test_add_AddsTwoNumbers()` 存在于 `MyContractTest` 中以测试 `add` 方法。
      - `function test_supply_UsersCanSupplyTokens()` 也存在于 `MyContractTest` 中以测试 `supply` 方法。
      - 这种方法的好处是测试输出按被测试的合约分组，这使得快速查看失败位置更容易。

1. 所有测试的一些通用指南：

   - 测试合约/函数应按被测试合约中的原始函数顺序编写。
   - 测试同一函数的所有单元测试应连续存在于测试文件中。
   - 测试合约可以继承任何你想要的辅助合约。例如，测试 `MyContract` 的 `contract MyContractTest` 可能继承自 forge-std 的 `Test`，以及你自己的 `TestUtilities` 辅助合约。

1. 集成测试应存在于相同的 `test` 目录中，并具有清晰的命名约定。这些可以在专用文件中，或者在与现有单元测试相关的测试文件中。

1. 保持测试命名的
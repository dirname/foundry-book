## 项目布局

Forge 在项目结构上非常灵活。默认结构如下：

```ignore
{{#include ../output/hello_foundry/tree-with-files:output}}
```

- 你可以使用 `foundry.toml` 配置 Foundry 的行为。
- 重映射在 `remappings.txt` 中指定。
- 合约的默认目录是 `src/`。
- 测试的默认目录是 `test/`，其中任何以 `test` 开头的函数都被视为测试。
- 依赖项作为 git 子模块存储在 `lib/` 中。

你可以分别使用 `--lib-paths` 和 `--contracts` 标志来配置 Forge 查找依赖项和合约的位置。或者你可以在 `foundry.toml` 中进行配置。

结合重映射，这为你提供了支持其他工具链（如 Hardhat 和 Truffle）项目结构所需的灵活性。

为了自动支持 Hardhat，你还可以传递 `--hh` 标志，该标志设置以下标志：`--lib-paths node_modules --contracts contracts`。
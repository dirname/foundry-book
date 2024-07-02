## 创建一个新项目

要使用 Foundry 开始一个新项目，请使用 [`forge init`](../reference/forge/forge-init.md)：

```sh
{{#include ../output/hello_foundry/forge-init:command}}
```

这将使用默认模板创建一个名为 `hello_foundry` 的新目录。这还会初始化一个新的 `git` 仓库。

如果你想使用不同的模板创建一个新项目，可以传递 `--template` 标志，如下所示：

```sh
$ forge init --template https://github.com/foundry-rs/forge-template hello_template
```

现在，让我们看看默认模板是什么样子的：

```sh
$ cd hello_foundry
{{#include ../output/hello_foundry/tree:all}}
```

默认模板带有一个已安装的依赖项：Forge 标准库。这是用于 Foundry 项目的首选测试库。此外，模板还带有一个空的入门合约和一个简单的测试。

让我们构建项目：

```sh
{{#include ../output/hello_foundry/forge-build:all}}
```

并运行测试：

```sh
{{#include ../output/hello_foundry/forge-test:all}}
```

你会注意到两个新目录已经出现：`out` 和 `cache`。

`out` 目录包含你的合约工件，例如 ABI，而 `cache` 由 `forge` 使用，仅重新编译必要的部分。
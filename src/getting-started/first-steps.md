## Foundry 入门

本节概述了 `forge` 命令行工具。我们演示如何创建一个新项目、编译和测试它。

要使用 Foundry 启动一个新项目，请使用 [`forge init`](../reference/forge/forge-init.md)：

```sh
{{#include ../output/hello_foundry/forge-init:command}}
```

让我们看看 `forge` 为我们生成了什么：

```sh
$ cd hello_foundry
{{#include ../output/hello_foundry/tree:all}}
```

我们可以使用 [`forge build`](../reference/forge/forge-build.md) 构建项目：

```sh
{{#include ../output/hello_foundry/forge-build:all}}
```

并使用 [`forge test`](../reference/forge/forge-test.md) 运行测试：

```sh
{{#include ../output/hello_foundry/forge-test:all}}
```
<br>

> 💡 **提示**
> 
> 你可以随时通过在末尾添加 `--help` 来打印任何子命令（或其子命令）的帮助信息。

如果你是视觉学习者，可以观看[这些](../tutorials/learn-foundry.md)入门教程。
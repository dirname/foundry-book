## Chisel 概述

Chisel 是 Foundry 附带的高级 Solidity REPL。它可以在本地或分叉网络上快速测试 Solidity 代码片段的行为。

Chisel 是 Foundry 套件的一部分，与 `forge`、`cast` 和 `anvil` 一起安装。如果您还没有安装 Foundry，请参阅 [Foundry 安装](../getting-started/installation.md)。

> 注意：如果您安装了旧版本的 Foundry，则需要重新安装 `foundryup` 以便下载 Chisel。

### 如何使用 Chisel

要使用 Chisel，只需输入 `chisel`。然后，开始编写 Solidity 代码！Chisel 将对每个输入提供详细的反馈。

Chisel 可以在 Foundry 项目内部和外部使用。如果在 Foundry 项目根目录中执行二进制文件，Chisel 将继承项目的配置选项。

要查看可用命令，请在 REPL 中输入 `!help`。

> 📚 **参考**
>
> 有关 Chisel 及其功能的深入信息，请参阅 [`chisel` 参考](../reference/chisel/)。
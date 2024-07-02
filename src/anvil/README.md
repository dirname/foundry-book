## Anvil 概述

Anvil 是 Foundry 附带的一个本地测试网节点。你可以使用它来测试从前端调用的合约或通过 RPC 进行交互。

Anvil 是 Foundry 套件的一部分，与 `forge`、`cast` 和 `chisel` 一起安装。如果你还没有安装 Foundry，请参见 [Foundry 安装](../getting-started/installation.md)。

> 注意：如果你安装了旧版本的 Foundry，你需要重新安装 `foundryup` 以便下载 Anvil。

### 如何使用 Anvil

要使用 Anvil，只需输入 `anvil`。你应该会看到一组可用的账户和私钥，以及节点正在监听的地址和端口。

Anvil 具有高度的可配置性。你可以运行 `anvil -h` 来查看所有配置选项。

一些基本选项包括：

```bash
# 要生成和配置的开发账户数量。[默认值：10]
anvil -a, --accounts <ACCOUNTS>

# 使用的 EVM 硬分叉。[默认值：latest]
anvil --hardfork <HARDFORK>

# 监听的端口号。[默认值：8545]
anvil  -p, --port <PORT>
```

> 📚 **参考**
>
> 请参见 [`anvil` 参考](../reference/anvil/) 以获取有关 Anvil 及其功能的深入信息。
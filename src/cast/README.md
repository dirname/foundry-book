## Cast 概述

Cast 是 Foundry 的命令行工具，用于执行以太坊 RPC 调用。您可以通过命令行进行智能合约调用、发送交易或检索任何类型的链上数据！

### 如何使用 Cast

要使用 Cast，请运行 [`cast`](../reference/cast/cast.md) 命令，后跟一个子命令：

```bash
$ cast <subcommand>
```

#### 示例

让我们使用 `cast` 来检索 DAI 代币的总供应量：

```bash
{{#include ../output/cast/cast-call:all}}
```

`cast` 还提供了许多方便的子命令，例如用于解码 calldata：

```bash
{{#include ../output/cast/cast-4byte-decode:all}}
```

您还可以使用 `cast` 发送任意消息。以下是一个在两个 Anvil 账户之间发送消息的示例。

```bash
$ cast send --private-key <Your Private Key> 0x3c44cdddb6a900fa2b585dd299e03d12fa4293bc $(cast from-utf8 "hello world") --rpc-url http://127.0.0.1:8545/
```

<br>

> 📚 **参考**
> 
> 请参阅 [`cast` 参考](../reference/cast/) 以获取所有可用子命令的完整概述。
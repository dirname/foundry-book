## chisel

### NAME

`chisel` - 在 REPL 环境中测试 Solidity 输入并接收详细反馈。

### SYNOPSIS

`chisel` [*options*]

#### 子命令 (bin)

1. `chisel list`
    * 显示存储在 `~/.foundry/cache/chisel` 中的所有缓存会话。
1. `chisel load <id>`
    * 如果存在 `id = <id>` 的缓存会话，启动 REPL 并加载相应的会话。
1. `chisel view <id>`
    * 如果存在 `id = <id>` 的缓存会话，显示会话的 REPL 合约的源代码。
1. `chisel clear-cache`
    * 删除 `~/.foundry/cache/chisel` 目录中的所有缓存文件。这些会话无法恢复，因此请谨慎使用此命令。

#### 标志

查看 `man chisel` 或 `chisel --help` 以获取所有可用的环境配置标志。

### DESCRIPTION

Chisel 是一个 Solidity REPL（即“读取-求值-打印循环”），允许开发人员编写和测试 Solidity 代码片段。它提供了一个交互式环境来编写和执行 Solidity 代码，以及一组内置命令来处理和调试代码。这使得它成为一个有用的工具，可以快速测试和试验 Solidity 代码，而无需启动沙盒 foundry 测试套件。

### 用法

要打开 chisel，只需执行 `chisel` 二进制文件。

从那里，输入有效的 Solidity 代码。除了命令之外，chisel 提示符有两种输入：
1. 表达式
    * 表达式是返回值或可以单独求值的语句。例如，`1 << 8` 是一个表达式，它将求值为一个值为 `256` 的 `uint256`。表达式将立即求值，并且在会话状态中不会持久存在。
    * 示例：
        * `address(0).balance`
        * `abi.encode(256, bytes32(0), "Chisel!")`
        * `myViewFunc(128)`
        * ...
1. 语句
    * 语句是旨在在会话状态中持久存在的代码片段。语句包括变量定义、调用返回值的非状态突变函数以及合约、函数、事件、错误、映射或结构定义。如果希望将表达式作为语句求值，可以在末尾附加一个分号（`;`）。
    * 示例：
        * `uint256 a = 0xa57b`
        * `myStateMutatingFunc(128)` || `myViewFunc(128);` <- 注意 `;`
        * ```solidity
          function hash64(
            bytes32 _a,
            bytes32 _b
          ) internal pure returns (bytes32 _hash) { 
              assembly {
                  // 将我们要哈希的 64 字节存储在暂存空间中
                  mstore(0x00, _a)
                  mstore(0x20, _b)

                  // 哈希暂存空间中的内存
                  // 并将结果赋值给 `_hash`
                  _hash := keccak256(0x00, 0x40)
              }
          }
          ```
        * `event ItHappened(bytes32 indexed hash)`
        * `struct Complex256 { uint256 re; uint256 im; }`
        * ...
#### 可用命令

```text
{{#include ../../output/chisel/help:output}}
```

**General**

`!help` | `!h`

显示所有命令。

`!quit` | `!q`

退出 Chisel。

`!exec <command> [args]` | `!e <command> [args]`

执行 shell 命令并打印输出。

示例：

```sh
➜ !e ls
CHANGELOG.md
LICENSE
README.md
TESTS.md
artifacts
cache
contracts
crytic-export
deploy
deploy-config
deployments
dist
echidna.yaml
forge-artifacts
foundry.toml
hardhat.config.ts
layout-lock.json
node_modules
package.json
scripts
slither.config.json
slither.db.json
src
tasks
test-case-generator
tsconfig.build.json
tsconfig.build.tsbuildinfo
tsconfig.json
```

**Session**

`!clear` | `!c`

清除当前会话源代码。

在底层，每个 Chisel 会话都有一个底层合约，随着输入语句的增加而改变。此命令清除该合约并将会话重置为默认状态。

`!source` | `!so`

显示当前会话的源代码。

如上所述，每个 Chisel 会话都有一个底层合约。此命令将显示该合约的源代码。

`!save [id]` | `!s [id]`

将当前会话保存到缓存。

Chisel 允许缓存会话，如果您在 Chisel 中测试更复杂的逻辑或在稍后时间返回会话，这非常有用。所有缓存的 Chisel 会话都存储在 `~/.foundry/cache/chisel` 中。

如果未提供 `id` 参数，Chisel 将自动为正在保存的会话分配一个数字 ID。

`!load <id>` | `!l <id>`

从缓存加载以前的会话 ID。

此命令将从缓存加载以前缓存的会话。除了会话源代码外，所有环境设置也将加载。`id` 参数必须对应于 `~/.foundry/cache/chisel` 目录中现有的缓存会话。

`!list` | `!ls`

列出所有缓存的会话。

此命令将显示 `~/.foundry/cache/chisel` 目录中的所有缓存 chisel 会话。

`!clearcache` | `!cc`

清除所有存储的会话的 chisel 缓存。

删除 `~/.foundry/cache/chisel` 目录中的所有缓存文件。这些会话无法恢复，因此请谨慎使用此命令。

`!export` | `!ex`

将当前会话源代码导出到脚本文件。

如果从 foundry 项目的根目录执行 `chisel`，可以将当前会话导出到项目 `scripts` 目录中的 foundry 脚本。

`!fetch <addr> <name>` | `!fe <addr> <name>`

从 Etherscan 获取已验证合约的接口。

此命令将尝试从 Etherscan API 解析位于 `<addr>` 的已验证合约的接口。如果成功，接口将以 `<name>` 插入到会话源代码中。

目前，只能获取以太坊主网上已验证合约的接口。未来，Chisel 将支持从多个 Etherscan 支持的链获取接口。

`!edit`

在编辑器中打开当前会话的 `run()` 函数。

chisel 将使用 `$EDITOR` 环境变量中定义的编辑器。

**Environment**

`!fork <url>` | `!f <url>`

为当前会话分叉一个 RPC。不提供参数则返回本地网络。

尝试分叉提供的 RPC 的状态。如果未提供 URL，则返回使用空白本地开发网络状态。

`!traces` | `!t`

为当前会话启用/禁用跟踪。

启用跟踪后，在每个语句插入后将打印 foundry 风格的调用跟踪和日志。

**Debug**

`!memdump` | `!md`

转储当前状态的原始内存。

尝试在 REPL 合约的 `run` 函数执行完最后一条指令后，转储机器状态的原始内存。

`!stackdump` | `!sd`

转储当前状态的原始堆栈。

尝试在 REPL 合约的 `run` 函数执行完最后一条指令后，转储机器状态的原始堆栈。

`!rawstack <var>` | `!rs <var>`

显示变量的堆栈分配的原始值。对于长度大于 32 字节的变量，这将显示其内存指针。

当您希望查看长度小于 32 字节的变量的完整原始堆栈分配时，此命令非常有用。

示例：

```sh
➜ address addr
➜ assembly {
    addr := not(0)
}
➜ addr
Type: address
└ Data: 0xffffffffffffffffffffffffffffffffffffffff
➜ !rs addr
Type: bytes32
└ Data: 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
➜ 
```
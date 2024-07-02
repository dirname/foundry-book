```markdown
# chisel

快速、实用且详细的 Solidity REPL

```bash
$ chisel --help
Usage: chisel [OPTIONS] [COMMAND]

Commands:
  list         列出所有缓存的会话
  load         加载一个缓存的会话
  view         查看缓存会话的源代码
  clear-cache  清除缓存目录中的所有缓存 chisel 会话
  help         打印此消息或给定子命令的帮助信息

Options:
  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

  -V, --version
          打印版本信息

REPL 选项:
      --prelude <PRELUDE>
          指向包含 Solidity 文件的目录的路径，或单个 Solidity 文件的路径。
          
          这些文件将在 REPL 的顶层之前进行评估，因此可以作为 prelude 使用。

      --no-vm
          禁用默认的 `Vm` 导入。
          
          如果 Solc 版本小于 0.6.2，默认情况下会禁用此导入。

缓存选项:
      --force
          清除缓存和 artifacts 文件夹并重新编译

构建选项:
      --no-cache
          禁用缓存

      --skip <SKIP>...
          跳过构建名称包含给定过滤器的文件。
          
          `test` 和 `script` 是 `.t.sol` 和 `.s.sol` 的别名。

链接器选项:
      --libraries <LIBRARIES>
          设置预链接的库
          
          [env: DAPP_LIBRARIES=]

编译器选项:
      --ignored-error-codes <ERROR_CODES>
          忽略 solc 警告的错误代码

      --deny-warnings
          警告将触发编译器错误

      --no-auto-detect
          不要自动检测 `solc` 版本

      --use <SOLC_VERSION>
          指定用于构建的 solc 版本，或本地 solc 的路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不要访问网络。
          
          缺失的 solc 版本将不会被安装。

      --via-ir
          使用 Yul 中间表示编译管道

      --no-metadata
          不要在字节码中附加任何元数据。
          
          这相当于将 `bytecode_hash` 设置为 `none` 并将 `cbor_metadata` 设置为 `false`。

      --silent
          启动时不打印任何内容

      --ast
          在编译器输出中包含 AST 的 JSON

      --evm-version <VERSION>
          目标 EVM 版本

      --optimize
          激活 Solidity 优化器

      --optimizer-runs <RUNS>
          优化器运行的次数

      --extra-output <SELECTOR>...
          在合约的 artifact 中包含的额外输出。
          
          示例键：evm.assembly、ewasm、ir、irOptimized、metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值：metadata、ir、irOptimized、ewasm、evm.assembly

项目选项:
  -o, --out <PATH>
          合约 artifacts 文件夹的路径

      --revert-strings <REVERT>
          回退字符串配置。
          
          可能的值为 "default"、"strip"（移除）、"debug"（Solidity 生成的回退字符串）和 "verboseDebug"

      --build-info
          生成构建信息文件

      --build-info-path <PATH>
          构建信息文件将写入的目录的输出路径

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在仓库中），或当前工作目录。

  -C, --contracts <PATH>
          合约源目录

  -R, --remappings <REMAPPINGS>
          项目的 remappings

      --remappings-env <ENV>
          从环境获取项目的 remappings

      --cache-path <PATH>
          编译器缓存的路径

      --lib-paths <PATH>
          库文件夹的路径

      --hardhat
          使用 Hardhat 风格的项目布局。
          
          这相当于使用：`--contracts contracts --lib-paths node_modules`。
          
          [aliases: hh]

      --config-path <FILE>
          配置文件的路径

EVM 选项:
  -f, --fork-url <URL>
          通过远程端点获取状态，而不是从空状态开始。
          
          如果你想从特定区块号获取状态，请参见 --fork-block-number。
          
          [aliases: rpc-url]

      --fork-block-number <BLOCK>
          通过远程端点从特定区块号获取状态。
          
          请参见 --fork-url。

      --fork-retries <RETRIES>
          重试次数。
          
          请参见 --fork-url。

      --fork-retry-backoff <BACKOFF>
          遇到错误时的初始重试退避。
          
          请参见 --fork-url。

      --no-storage-caching
          显式禁用 RPC 缓存。
          
          所有存储槽完全从端点读取。
          
          此标志会覆盖项目的配置文件。
          
          请参见 --fork-url。

      --initial-balance <BALANCE>
          部署测试合约的初始余额

      --sender <ADDRESS>
          将执行测试的地址

      --ffi
          启用 FFI cheatcode

      --always-use-create-2-factory
          在所有情况下（包括测试和非广播脚本）使用 create 2 工厂

  -v, --verbosity...
          EVM 的详细程度。
          
          多次传递以增加详细程度（例如 -v、-vv、-vvv）。
          
          详细程度级别：
          - 2：打印所有测试的日志
          - 3：打印失败测试的执行 traces
          - 4：打印所有测试的执行 traces，以及失败测试的设置 traces
          - 5：打印所有测试的执行和设置 traces

Fork 配置:
      --compute-units-per-second <CUPS>
          设置此提供者假设可用的计算单元每秒数量
          
          默认值：330
          
          请参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>

      --no-rpc-rate-limit
          禁用此节点提供者的速率限制。
          
          请参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>
          
          [aliases: no-rate-limit]

执行环境配置:
      --gas-limit <GAS_LIMIT>
          区块 gas 限制

      --code-size-limit <CODE_SIZE>
          EIP-170：合约代码大小限制（字节）。增加此值有助于测试。默认值为 0x6000（~25kb）

      --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [aliases: chain-id]

      --gas-price <GAS_PRICE>
          gas 价格

      --block-base-fee-per-gas <FEE>
          区块中的基础费用
          
          [aliases: base-fee]

      --tx-origin <ADDRESS>
          交易发起者

      --block-coinbase <ADDRESS>
          区块的 coinbase

      --block-timestamp <TIMESTAMP>
          区块的时间戳

      --block-number <BLOCK>
          区块号

      --block-difficulty <DIFFICULTY>
          区块难度

      --block-prevrandao <PREVRANDAO>
          区块的 prevrandao 值。注意：合并前此字段为 mix_hash

      --block-gas-limit <GAS_LIMIT>
          区块 gas 限制

      --memory-limit <MEMORY_LIMIT>
          EVM 执行的内存限制（字节）。如果超过此限制，将抛出 `MemoryLimitOOG` 结果。
          
          默认值为 128MiB。

      --disable-block-gas-limit
          是否禁用区块 gas 限制检查
          
          [aliases: no-gas-limit]

      --isolate
          是否启用调用的隔离。在隔离模式下，所有顶级调用都作为单独的交易在单独的 EVM 上下文中执行，从而实现更精确的 gas 计算和交易状态变化
```
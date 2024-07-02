# forge coverage

生成覆盖率报告

```bash
$ forge coverage --help
Usage: forge coverage [OPTIONS]

Options:
      --report <REPORT>
          用于覆盖率的报告类型。
          
          此标志可以多次使用。
          
          [默认值: summary]
          [可能的值: summary, lcov, debug, bytecode]

      --ir-minimum
          启用 viaIR 并使用最小优化
          
          这可以修复大多数“堆栈太深”错误，同时产生相对准确的源映射。

  -r, --report-file <PATH>
          输出报告的路径。
          
          如果未指定，报告将存储在项目的根目录中。

      --include-libs
          是否在覆盖率报告中包含库

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Test options:
      --debug <TEST_FUNCTION>
          在调试器中运行测试。
          
          传递给此标志的参数是要运行的测试函数的名称，其工作方式与 --match-test 相同。
          
          如果多个测试符合指定的条件，必须添加额外的过滤器，直到找到一个测试（参见 --match-contract 和 --match-path）。
          
          匹配的测试将在调试器中打开，无论测试结果如何。
          
          如果匹配的测试是模糊测试，则会在第一个失败案例中打开调试器。如果模糊测试没有失败，则会在最后一个模糊案例中打开调试器。
          
          有关更精细的控制，请参见 forge run。

      --gas-report
          打印 gas 报告
          
          [环境变量: FORGE_GAS_REPORT=]

      --allow-failure
          即使测试失败也退出代码 0
          
          [环境变量: FORGE_ALLOW_FAILURE=]

      --fail-fast
          在第一次失败后停止运行测试

      --etherscan-api-key <KEY>
          Etherscan（或等效）API 密钥
          
          [环境变量: ETHERSCAN_API_KEY=]

      --fuzz-seed <FUZZ_SEED>
          设置用于生成模糊测试随机性的种子

      --fuzz-runs <RUNS>
          [环境变量: FOUNDRY_FUZZ_RUNS=]

      --fuzz-input-file <FUZZ_INPUT_FILE>
          从文件中重新运行模糊测试失败案例

      --max-threads <MAX_THREADS>
          使用的最大并发线程数。默认值是可用 CPU 的数量

Display options:
  -j, --json
          以 JSON 格式输出测试结果

  -l, --list
          列出测试而不是运行它们

      --summary
          打印测试摘要表

      --detailed
          打印详细的测试摘要表

Test filtering:
      --match-test <REGEX>
          仅运行匹配指定正则表达式的测试函数
          
          [别名: mt]

      --no-match-test <REGEX>
          仅运行不匹配指定正则表达式的测试函数
          
          [别名: nmt]

      --match-contract <REGEX>
          仅运行匹配指定正则表达式的合约中的测试
          
          [别名: mc]

      --no-match-contract <REGEX>
          仅运行不匹配指定正则表达式的合约中的测试
          
          [别名: nmc]

      --match-path <GLOB>
          仅运行匹配指定全局模式源文件中的测试
          
          [别名: mp]

      --no-match-path <GLOB>
          仅运行不匹配指定全局模式源文件中的测试
          
          [别名: nmp]

EVM options:
  -f, --fork-url <URL>
          通过远程端点获取状态，而不是从空状态开始。
          
          如果要从特定区块号获取状态，请参见 --fork-block-number。
          
          [别名: rpc-url]

      --fork-block-number <BLOCK>
          通过远程端点从特定区块号获取状态。
          
          参见 --fork-url。

      --fork-retries <RETRIES>
          重试次数。
          
          参见 --fork-url。

      --fork-retry-backoff <BACKOFF>
          遇到错误时的初始重试退避。
          
          参见 --fork-url。

      --no-storage-caching
          显式禁用 RPC 缓存。
          
          所有存储槽完全从端点读取。
          
          此标志覆盖项目的配置文件。
          
          参见 --fork-url。

      --initial-balance <BALANCE>
          部署测试合约的初始余额

      --sender <ADDRESS>
          执行测试的地址

      --ffi
          启用 FFI 作弊码

      --always-use-create-2-factory
          在所有情况下使用 create 2 工厂，包括测试和非广播脚本

  -v, --verbosity...
          EVM 的详细程度。
          
          多次传递以增加详细程度（例如 -v, -vv, -vvv）。
          
          详细程度级别：
          - 2: 打印所有测试的日志
          - 3: 打印失败测试的执行 traces
          - 4: 打印所有测试的执行 traces 和失败测试的设置 traces
          - 5: 打印所有测试的执行和设置 traces

Fork config:
      --compute-units-per-second <CUPS>
          设置此提供者假设的每秒可用计算单元数
          
          默认值: 330
          
          参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>

      --no-rpc-rate-limit
          禁用此节点提供者的速率限制。
          
          参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>
          
          [别名: no-rate-limit]

Executor environment config:
      --gas-limit <GAS_LIMIT>
          区块 gas 限制

      --code-size-limit <CODE_SIZE>
          EIP-170: 合约代码大小限制（字节）。增加此值有助于测试。默认值为 0x6000（~25kb）

      --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [别名: chain-id]

      --gas-price <GAS_PRICE>
          gas 价格

      --block-base-fee-per-gas <FEE>
          区块的基础费用
          
          [别名: base-fee]

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
          
          [别名: no-gas-limit]

      --isolate
          是否启用调用的隔离。在隔离模式下，所有顶级调用作为单独的交易在单独的 EVM 上下文中执行，实现更精确的 gas 计算和交易状态变化

Cache options:
      --force
          清除缓存和 artifacts 文件夹并重新编译

Build options:
      --no-cache
          禁用缓存

      --skip <SKIP>...
          跳过构建名称包含给定过滤器的文件。
          
          `test` 和 `script` 是 `.t.sol` 和 `.s.sol` 的别名。

Linker options:
      --libraries <LIBRARIES>
          设置预链接的库
          
          [环境变量: DAPP_LIBRARIES=]

Compiler options:
      --ignored-error-codes <ERROR_CODES>
          忽略 solc 警告的错误代码

      --deny-warnings
          警告将触发编译器错误

      --no-auto-detect
          不自动检测 `solc` 版本

      --use <SOLC_VERSION>
          指定用于构建的 solc 版本或本地 solc 路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不访问网络。
          
          缺失的 solc 版本不会被安装。

      --via-ir
          使用 Yul 中间表示编译管道

      --no-metadata
          不在字节码中附加任何元数据。
          
          这相当于将 `bytecode_hash` 设置为 `none` 并将 `cbor_metadata` 设置为 `false`。

      --silent
          启动时不打印任何内容

      --ast
          在编译器输出中包含 AST 作为 JSON

      --evm-version <VERSION>
          目标 EVM 版本

      --optimize
          激活 Solidity 优化器

      --optimizer-runs <RUNS>
          优化器运行次数

      --extra-output <SELECTOR>...
          在合约的 artifact 中包含额外的输出。
          
          示例键: evm.assembly, ewasm, ir, irOptimized, metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值: metadata, ir, irOptimized, ewasm, evm.assembly

Project options:
  -o, --out <PATH>
          合约 artifacts 文件夹的路径

      --revert-strings <REVERT>
          回退字符串配置。
          
          可能的值为 "default"、"strip"（移除）、"debug"（Solidity 生成的回退字符串）和 "verboseDebug"

      --build-info
          生成构建信息文件

      --build-info-path <PATH>
          构建信息文件的输出路径

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在 Git 仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源目录

  -R, --remappings <REMAPPINGS>
          项目的 remappings

      --remappings-env <ENV>
          从环境变量中获取项目的 remappings

      --cache-path <PATH>
          编译器缓存的路径

      --lib-paths <PATH>
          库文件夹的路径

      --hardhat
          使用 Hardhat 风格的
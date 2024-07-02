```markdown
# forge script

运行智能合约作为脚本，构建可以发送到链上的交易

```bash
$ forge script --help
Usage: forge script [OPTIONS] <PATH> [ARGS]...

Arguments:
  <PATH>
          你要运行的合约。可以是文件路径或合约名称。
          
          如果同一个文件中存在多个合约，你必须使用
          --target-contract 指定目标合约。

  [ARGS]...
          传递给脚本函数的参数

Options:
      --target-contract <CONTRACT_NAME>
          你要运行的合约名称
          
          [aliases: tc]

  -s, --sig <SIG>
          你要在合约中调用的函数的签名，或原始调用数据
          
          [default: run()]

      --priority-gas-price <PRICE>
          EIP1559 交易的每单位 gas 的最大优先费用
          
          [env: ETH_PRIORITY_GAS_PRICE=]

      --legacy
          使用传统交易而不是 EIP1559 交易。
          
          对于没有 EIP1559 的常见网络会自动启用。

      --broadcast
          广播交易

      --batch-size <BATCH_SIZE>
          交易的批量大小。
          
          如果批量处理不可用或启用了 `--slow`，则忽略并设置为 1。
          
          [default: 100]

      --skip-simulation
          跳过链上模拟

  -g, --gas-estimate-multiplier <GAS_ESTIMATE_MULTIPLIER>
          相对百分比乘以 gas 估计值
          
          [default: 130]

      --unlocked
          通过 `eth_sendTransaction` 发送，使用 `--from` 参数或 `$ETH_FROM` 作为发送者

      --resume
          恢复提交之前失败或超时的交易。
          
          它不会再次模拟脚本，并且期望 nonce 保持不变。
          
          例如：如果交易 N 的 nonce 为 22，那么账户应该有 nonce 22，否则会失败。

      --multi
          如果存在，--resume 或 --verify 将被假定为多链部署

      --debug
          在调试器中打开脚本。
          
          优先于 broadcast。

      --slow
          确保交易在其前一个交易被确认并成功后才发送

      --non-interactive
          禁用部署大合约时可能出现的交互提示。
          
          有关合约大小限制的更多信息，请参见 EIP-170：
          <https://eips.ethereum.org/EIPS/eip-170>

      --etherscan-api-key <KEY>
          Etherscan（或等效）API 密钥
          
          [env: ETHERSCAN_API_KEY=]

      --verify
          验证脚本收据中找到的所有合约（如果有）

      --json
          以 JSON 格式输出结果

      --with-gas-price <PRICE>
          传统交易的 gas 价格，或 EIP1559 交易的每单位 gas 的最大费用
          
          [env: ETH_GAS_PRICE=]

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

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
          
          [env: DAPP_LIBRARIES=]

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
          
          示例键：evm.assembly、ewasm、ir、irOptimized、metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值：metadata、ir、irOptimized、ewasm、evm.assembly

Project options:
  -o, --out <PATH>
          合约 artifacts 文件夹的路径

      --revert-strings <REVERT>
          回退字符串配置。
          
          可能的值为 "default"、"strip"（移除）、"debug"（Solidity 生成的回退字符串）和 "verboseDebug"

      --build-info
          生成构建信息文件

      --build-info-path <PATH>
          构建信息文件将写入的目录输出路径

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在 Git 仓库中），或当前工作目录。

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

Wallet options - raw:
  -a, --froms [<ADDRESSES>...]
          发送者账户
          
          [env: ETH_FROM=]

  -i, --interactives <NUM>
          打开一个交互提示以输入你的私钥。
          
          取一个值表示要输入的密钥数量。
          
          [default: 0]

      --private-keys <RAW_PRIVATE_KEYS>
          使用提供的私钥

      --private-key <RAW_PRIVATE_KEY>
          使用提供的私钥

      --mnemonics <MNEMONICS>
          使用指定路径的助记词文件的助记词短语

      --mnemonic-passphrases <PASSPHRASE>
          使用 BIP39 助记词的密码短语

      --mnemonic-derivation-paths <PATH>
          钱包派生路径。
          
          适用于 --mnemonic-path 和硬件钱包。

      --mnemonic-indexes <INDEXES>
          使用给定助记词索引的私钥。
          
          可与 --mnemonics、--ledger、--aws 和 --trezor 一起使用。
          
          [default: 0]

Wallet options - keystore:
      --keystore <PATHS>
          使用给定文件夹或文件中的 keystore
          
          [env: ETH_KEYSTORE=]
          [aliases: keystores]

      --account <ACCOUNT_NAMES>
          使用默认 keystores 文件夹（~/.foundry/keystores）中按文件名指定的 keystore
          
          [env: ETH_KEYSTORE_ACCOUNT=]
          [aliases: accounts]

      --password <PASSWORDS>
          keystore 密码。
          
          与 --keystore 一起使用。

      --password-file <PATHS>
          keystore 密码文件路径。
          
          与 --keystore 一起使用。
          
          [env: ETH_PASSWORD=]

Wallet options - hardware wallet:
  -l, --ledger
          使用 Ledger 硬件钱包

  -t, --trezor
          使用 Trezor 硬件钱包

Wallet options - remote:
      --aws
          使用 AWS Key Management Service

EVM options:
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
          
          所有存储槽都完全从端点读取。
          
          此标志覆盖项目的配置文件。
          
          请参见 --fork-url。

      --initial-balance <BALANCE>
          部署测试合约的初始余额

      --sender <ADDRESS>
          执行测试的地址

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

Fork config:
      --compute-units-per-second <CUPS>
          设置此提供商假设可用的每秒计算单位数
          
          默认值：330
          
          请参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>

      --no-rpc-rate-limit
          禁用此节点提供商的速率限制。
          
          请参见 --fork-url 和
          <https://docs.alchemy.com/reference/compute-units#what-are-cups-compute-units-per-second>
          
          [aliases: no-rate-limit]

Executor environment config:
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
          区块的基础费用
          
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
          EVM 执行的每单位内存限制（字节）。如果超过此限制，将抛出 `MemoryLimitOOG` 结果。
          
          默认值为 128MiB。

      --disable-block-gas-limit
          是否禁用区块 gas 限制检查
          
          [aliases: no-gas-limit]

      --isolate
          是否启用调用的隔离。在隔离模式下，所有顶级调用都作为单独的交易在单独的 EVM 上下文中执行，实现更精确的 gas 会计和交易状态更改

      --retries <RETRIES>
          验证重试次数
          
          [default: 5]

      --delay <DELAY>
          验证尝试之间的可选延迟（秒）
          
          [default: 5]

Verifier options:
      --verifier <VERIFIER>
          使用的合约验证提供商
          
          [default: etherscan]
          [possible values: etherscan, sourcify, blockscout, oklink]

      --verifier-url <VERIFIER_URL>
          如果使用自定义提供商，则设置验证 URL
          
          [env: VERIFIER_URL=]
```
```
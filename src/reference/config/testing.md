## 测试

与 `forge test` 行为相关的配置。

**章节**

- [通用](#general)
- [模糊测试](#fuzz)
- [不变性测试](#invariant)


### 通用

##### `verbosity`

- 类型: 整数
- 默认值: 0
- 环境变量: `FOUNDRY_VERBOSITY` 或 `DAPP_VERBOSITY`

测试期间使用的详细级别。

- **级别 2 (`-vv`)**: 测试期间发出的日志也会显示。
- **级别 3 (`-vvv`)**: 失败测试的堆栈跟踪也会显示。
- **级别 4 (`-vvvv`)**: 所有测试的堆栈跟踪都会显示，并且失败测试的设置跟踪也会显示。
- **级别 5 (`-vvvvv`)**: 堆栈跟踪和设置跟踪总是显示。

##### `ffi`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_FFI` 或 `DAPP_FFI`

是否启用 `ffi` 作弊码。

**警告:** 启用此作弊码会对您的项目安全产生影响，因为它允许测试在您的计算机上执行任意程序。

##### `sender`

- 类型: 字符串 (地址)
- 默认值: 0x1804c8AB1F12E6bbf3894d4083f33e07309d1f38
- 环境变量: `FOUNDRY_SENDER` 或 `DAPP_SENDER`

测试中的 `msg.sender` 值。

##### `tx_origin`

- 类型: 字符串 (地址)
- 默认值: 0x1804c8AB1F12E6bbf3894d4083f33e07309d1f38
- 环境变量: `FOUNDRY_TX_ORIGIN` 或 `DAPP_TX_ORIGIN`

测试中的 `tx.origin` 值。

##### `initial_balance`

- 类型: 字符串 (十六进制)
- 默认值: 0xffffffffffffffffffffffff
- 环境变量: `FOUNDRY_INITIAL_BALANCE` 或 `DAPP_INITIAL_BALANCE`

测试合约的初始余额，以 wei 为单位，用十六进制表示。

##### `block_number`

- 类型: 整数
- 默认值: 1
- 环境变量: `FOUNDRY_BLOCK_NUMBER` 或 `DAPP_BLOCK_NUMBER`

测试中的 `block.number` 值。

##### `chain_id`

- 类型: 整数
- 默认值: 31337
- 环境变量: `FOUNDRY_CHAIN_ID` 或 `DAPP_CHAIN_ID`

测试中的 `chainid` 操作码值。

##### `gas_limit`

- 类型: 整数或字符串
- 默认值: 9223372036854775807
- 环境变量: `FOUNDRY_GAS_LIMIT` 或 `DAPP_GAS_LIMIT`

每个测试用例的 gas 限制。

> ℹ️ **注意**
>
> 由于 Forge 依赖的一个限制，您**无法将 gas 限制**提高到默认值以上而不将值更改为字符串。
>
> 为了使用更高的 gas 限制，请使用字符串：
 ```toml
gas_limit = "18446744073709551615" # u64::MAX
```

##### `gas_price`

- 类型: 整数
- 默认值: 0
- 环境变量: `FOUNDRY_GAS_PRICE` 或 `DAPP_GAS_PRICE`

测试中的 gas 价格（以 wei 为单位）。

##### `block_base_fee_per_gas`

- 类型: 整数
- 默认值: 0
- 环境变量: `FOUNDRY_BLOCK_BASE_FEE_PER_GAS` 或 `DAPP_BLOCK_BASE_FEE_PER_GAS`

测试中的每 gas 基础费用（以 wei 为单位）。

##### `block_coinbase`

- 类型: 字符串 (地址)
- 默认值: 0x0000000000000000000000000000000000000000
- 环境变量: `FOUNDRY_BLOCK_COINBASE` 或 `DAPP_BLOCK_COINBASE`

测试中的 `block.coinbase` 值。

##### `block_timestamp`

- 类型: 整数
- 默认值: 1
- 环境变量: `FOUNDRY_BLOCK_TIMESTAMP` 或 `DAPP_BLOCK_TIMESTAMP`

测试中的 `block.timestamp` 值。

##### `block_difficulty`

- 类型: 整数
- 默认值: 0
- 环境变量: `FOUNDRY_BLOCK_DIFFICULTY` 或 `DAPP_BLOCK_DIFFICULTY`

测试中的 `block.difficulty` 值。

##### `gas_reports`

- 类型: 字符串数组 (合约名称)
- 默认值: ["*"]
- 环境变量: `FOUNDRY_GAS_REPORTS` 或 `DAPP_GAS_REPORTS`

要打印 gas 报告的合约。

##### `no_storage_caching`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_NO_STORAGE_CACHING` 或 `DAPP_NO_STORAGE_CACHING`

如果设置为 `true`，则测试中来自 RPC 端点的区块数据将不会被缓存。否则，数据会被缓存到 `$HOME/.foundry/cache/<chain id>/<block number>`。

##### `[rpc_storage_caching]`

`[rpc_storage_caching]` 块决定哪些 RPC 端点被缓存。

###### `rpc_storage_caching.chains`

- 类型: 字符串或字符串数组 (链名称)
- 默认值: all
- 环境变量: 无

决定哪些链被缓存。默认情况下，所有链都被缓存。

有效值为：

- "all"
- 链名称列表，例如 `["optimism", "mainnet"]`

###### `rpc_storage_caching.endpoints`

- 类型: 字符串或正则表达式模式数组 (匹配 URL)
- 默认值: remote
- 环境变量: 无

决定哪些 RPC 端点被缓存。默认情况下，只有远程端点被缓存。

有效值为：

- all
- remote (默认)
- 正则表达式模式列表，例如 `["localhost"]`

##### `eth_rpc_url`

- 类型: 字符串
- 默认值: 无
- 环境变量: `FOUNDRY_ETH_RPC_URL` 或 `DAPP_ETH_RPC_URL`

应使用的 RPC 服务器的 URL。

##### `etherscan_api_key`

- 类型: 字符串
- 默认值: 无
- 环境变量: `FOUNDRY_ETHERSCAN_API_KEY` 或 `DAPP_ETHERSCAN_API_KEY`

用于 RPC 调用的 Etherscan API 密钥。

##### `match-test`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_MATCH_TEST` 或 `DAPP_MATCH_TEST`

仅运行匹配正则表达式的测试方法。
等同于 `forge test --match-test <TEST_PATTERN>`

##### `no-match-test`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_NO_MATCH_TEST` 或 `DAPP_NO_MATCH_TEST`

仅运行不匹配正则表达式的测试方法。
等同于 `forge test --no-match-test <TEST_PATTERN_INVERSE>`

##### `match-contract`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_MATCH_CONTRACT` 或 `DAPP_MATCH_CONTRACT`

仅运行匹配正则表达式的合约中的测试方法。
等同于 `forge test --match-contract <CONTRACT_PATTERN>`

##### `no-match-contract`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_NO_MATCH_CONTRACT` 或 `DAPP_NO_MATCH_CONTRACT`

仅运行不匹配正则表达式的合约中的测试方法。
等同于 `forge test --no-match-contract <CONTRACT_PATTERN_INVERSE>`

##### `match-path`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_MATCH_PATH` 或 `DAPP_MATCH_PATH`

仅运行匹配路径的文件中的测试方法。
等同于 `forge test --match-path <PATH_PATTERN>`

##### `no-match-path`

- 类型: 正则表达式
- 默认值: 无
- 环境变量: `FOUNDRY_NO_MATCH_PATH` 或 `DAPP_NO_MATCH_PATH`

仅运行不匹配路径的文件中的测试方法。
等同于 `forge test --no-match-path <PATH_PATTERN_INVERSE>`

##### `block_gas_limit`

- 类型: 整数
- 默认值: 无
- 环境变量: `FOUNDRY_BLOCK_GAS_LIMIT` 或 `DAPP_BLOCK_GAS_LIMIT`

EVM 执行期间的 `block.gaslimit` 值。

##### `memory_limit`

- 类型: 整数
- 默认值: 33554432
- 环境变量: `FOUNDRY_MEMORY_LIMIT` 或 `DAPP_MEMORY_LIMIT`

EVM 的内存限制（以字节为单位）。

##### `names`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_NAMES` 或 `DAPP_NAMES`

打印编译的合约名称。

##### `sizes`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_SIZES` 或 `DAPP_SIZES`

打印编译的合约大小。

##### `rpc_endpoints`

- 类型: RPC 端点表
- 默认值: 无
- 环境变量: 无

此部分存在于配置文件之外，定义了一个 RPC 端点表，其中键指定了 RPC 端点的名称，值是 RPC 端点本身。

值可以是有效的 RPC 端点或对环境变量的引用（用 `${}` 包裹）。

这些 RPC 端点可以在测试和 Solidity 脚本中使用（参见 [`vm.rpc`](../../cheatcodes/rpc.md)）。

以下示例定义了一个名为 `optimism` 的端点和一个名为 `mainnet` 的端点，该端点引用了一个环境变量 `RPC_MAINNET`：

```toml
[rpc_endpoints]
optimism = "https://optimism.alchemyapi.io/v2/..."
mainnet = "${RPC_MAINNET}"
```

##### `prompt_timeout`

- 类型: 整数
- 默认值: 120
- 环境变量: `FOUNDRY_PROMPT_TIMEOUT`

`vm.prompt` 在超时前等待的秒数。

### 模糊测试

`[fuzz]` 部分的配置值。

##### `runs`

- 类型: 整数
- 默认值: 256
- 环境变量: `FOUNDRY_FUZZ_RUNS` 或 `DAPP_FUZZ_RUNS`

每个模糊测试用例执行的模糊运行次数。更高的值在测试速度的代价下提供更多结果的置信度。

##### `max_test_rejects`

- 类型: 整数
- 默认值: 65536
- 环境变量: `FOUNDRY_FUZZ_MAX_TEST_REJECTS`

在测试整体中止之前可能被拒绝的组合输入的最大数量。
“全局”过滤器适用于整个测试用例。如果测试用例被拒绝，整个测试用例将重新生成。

##### `seed`

- 类型: 字符串 (十六进制)
- 默认值: 无
- 环境变量: `FOUNDRY_FUZZ_SEED`

模糊测试 RNG 算法的可选种子。

##### `dictionary_weight`

- 类型: 整数 (0 到 100 之间)
- 默认值: 40
- 环境变量: `FOUNDRY_FUZZ_DICTIONARY_WEIGHT`

字典的权重。更高的字典权重将使模糊输入偏向“有趣的”值，例如边界值如 `type(uint256).max` 或来自您环境的合约地址。

##### `include_storage`

- 类型: 布尔值
- 默认值: true
- 环境变量: `FOUNDRY_FUZZ_INCLUDE_STORAGE`

指示是否包含存储值的标志。

##### `include_push_bytes`

- 类型: 布尔值
- 默认值: true
- 环境变量: `FOUNDRY_FUZZ_INCLUDE_PUSH_BYTES`

指示是否包含 push bytes 值的标志。

### 不变性测试

`[invariant]` 部分的配置值。

> ℹ️ **注意**
>
> `[invariant]` 部分的配置具有回退逻辑
> 对于常见的配置条目（`runs`, `seed`, `dictionary_weight` 等）。
>
> * 如果条目在任一部分中未设置，则将使用默认值。
> * 如果条目在 `[fuzz]` 部分中设置，但在 `[invariant]` 部分中未设置，这些值将自动设置为 `[fuzz]` 部分中指定的值。
> * 对于 `default` 以外的任何配置文件：
>     * 如果在 `[invariant]`（与 `[profile.default.invariant]` 相同）部分中至少设置了一个条目，则将使用 `[invariant]` 部分的值，包括默认值。
>     * 如果在 `[invariant]` 部分中未设置任何条目，但在 `[fuzz]`（与 `[profile.default.fuzz]` 相同）部分中设置了条目，则将使用 `[fuzz]` 部分的值。
>     * 如果上述情况均不适用，则将使用默认值。

##### `runs`

- 类型: 整数
- 默认值: 256
- 环境变量: `FOUNDRY_INVARIANT_RUNS`

每个不变性测试组必须执行的运行次数。另见 [fuzz.runs](#runs)

##### `depth`

- 类型: 整数
- 默认值: 500
- 环境变量: `FOUNDRY_INVARIANT_DEPTH`

在一次运行中尝试破坏不变性的调用次数。

##### `fail_on_revert`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_INVARIANT_FAIL_ON_REVERT`

如果发生回滚，则失败不变性模糊测试。

##### `call_override`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_INVARIANT_CALL_OVERRIDE`

在运行不变性测试时覆盖不安全的
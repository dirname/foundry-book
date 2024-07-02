## Solidity 编译器

与 Solidity 编译器行为相关的配置。

**章节**

- [通用](#general)
- [优化器](#optimizer)
- [模型检查器](#model-checker)

### 通用

与 Solidity 编译器行为相关的配置。

##### `remappings`

- 类型: 字符串数组 (重映射)
- 默认值: 无
- 环境变量: `FOUNDRY_REMAPPINGS` 或 `DAPP_REMAPPINGS`

一个重映射数组，格式为 `<名称>=<目标>`。

重映射将 Solidity 导入重定向到不同的目录。例如，以下重映射

```ignore
@openzeppelin/=node_modules/@openzeppelin/openzeppelin-contracts/
```

对于如下导入

```solidity
import "@openzeppelin/contracts/utils/Context.sol";
```

将变为

```solidity
import "node_modules/@openzeppelin/openzeppelin-contracts/contracts/utils/Context.sol";
```

##### `auto_detect_remappings`

- 类型: 布尔值
- 默认值: true
- 环境变量: `FOUNDRY_AUTO_DETECT_REMAPPINGS` 或 `DAPP_AUTO_DETECT_REMAPPINGS`

如果启用，Foundry 将自动尝试通过扫描 `libs` 文件夹来检测重映射。

如果设置为 `false`，则仅使用 `foundry.toml` 和 `remappings.txt` 中的重映射。

##### `allow_paths`

- 类型: 字符串数组 (路径)
- 默认值: 无
- 环境变量: `FOUNDRY_ALLOW_PATHS` 或 `DAPP_ALLOW_PATHS`

告诉 solc 允许从其他目录读取源文件。这主要与 `pnpm` 或类似管理的复杂工作区相关。

参见 [solc allowed-paths](https://docs.soliditylang.org/en/latest/path-resolution.html#allowed-paths)

##### `include_paths`

- 类型: 字符串数组 (路径)
- 默认值: 无
- 环境变量: `FOUNDRY_INCLUDE_PATHS` 或 `DAPP_INCLUDE_PATHS`

使额外的源目录可用于默认导入回调。如果你想导入位置不固定的合约，例如使用包管理器安装的第三方库，可以使用此选项。可以多次使用。仅当基本路径有非空值时才有效。

参见 [solc 路径解析](https://docs.soliditylang.org/en/latest/path-resolution.html)

##### `libraries`

- 类型: 字符串数组 (库)
- 默认值: 无
- 环境变量: `FOUNDRY_LIBRARIES` 或 `DAPP_LIBRARIES`

一个库数组，格式为 `<文件>:<库>:<地址>`，例如：`src/MyLibrary.sol:MyLibrary:0xfD88CeE74f7D78697775aBDAE53f9Da1559728E4`。

##### `solc_version`

- 类型: 字符串 (语义版本)
- 默认值: 无
- 环境变量: `FOUNDRY_SOLC_VERSION` 或 `DAPP_SOLC_VERSION`

如果指定，将覆盖自动检测系统（详见下文），并使用单一的 Solidity 编译器版本进行项目编译。

仅支持严格的版本（例如 `0.8.11` 有效，但 `^0.8.0` 无效）。

##### `auto_detect_solc`

- 类型: 布尔值
- 默认值: true
- 环境变量: `FOUNDRY_AUTO_DETECT_SOLC` 或 `DAPP_AUTO_DETECT_SOLC`

如果启用，Foundry 将自动尝试解析适当的 Solidity 编译器版本以编译你的项目。

如果设置了 `solc_version`，则忽略此键。

##### `offline`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_OFFLINE` 或 `DAPP_OFFLINE`

如果启用，Foundry 将不会尝试下载任何缺失的 solc 版本。

如果 `offline` 和 `auto-detect-solc` 都设置为 `true`，将自动检测所需的 solc 版本，但不会安装任何缺失的版本。

##### `ignored_warnings_from`

- 类型: 字符串数组 (文件路径)
- 默认值: 无
- 环境变量: `FOUNDRY_IGNORED_WARNINGS_FROM` 或 `DAPP_IGNORED_WARNINGS_FROM`

一个文件路径数组，编译过程中应忽略这些路径中的警告。当你有特定目录的文件产生已知警告，并且希望在不影响其他文件的情况下抑制这些警告时，这很有用。

数组中的每个条目应为目录路径或特定文件路径。例如：

`ignored_warnings_from = ["path/to/warnings/file1.sol", "path/to/warnings/file2.sol"]`

此配置将导致编译器忽略指定路径中的任何警告。

##### `ignored_error_codes`

- 类型: 整数/字符串数组
- 默认值: 对于源文件、SPDX 许可证标识符和合约大小为无，对于测试为无
- 环境变量: `FOUNDRY_IGNORED_ERROR_CODES` 或 `DAPP_IGNORED_ERROR_CODES`

一个 Solidity 编译器错误代码数组，编译过程中应忽略这些代码，例如警告。

有效值包括：

- `license`: 1878
- `code-size`: 5574
- `func-mutability`: 2018
- `unused-var`: 2072
- `unused-param`: 5667
- `unused-return`: 9302
- `virtual-interfaces`: 5815
- `missing-receive-ether`: 3628
- `shadowing`: 2519
- `same-varname`: 8760
- `unnamed-return`: 6321
- `unreachable`: 5740
- `pragma-solidity`: 3420
- `constructor-visibility`: 2462
- `init-code-size`: 3860
- `transient-storage`: 2394
- `too-many-warnings`: 4591

##### `deny_warnings`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_DENY_WARNINGS` 或 `DAPP_DENY_WARNINGS`

如果启用，Foundry 会将 Solidity 编译器警告视为错误，阻止将工件写入磁盘。

##### `evm_version`

- 类型: 字符串
- 默认值: paris
- 环境变量: `FOUNDRY_EVM_VERSION` 或 `DAPP_EVM_VERSION`

测试期间使用的 EVM 版本。值**必须**是 EVM 硬分叉名称，例如 `london`、`byzantium` 等。

##### `revert_strings`

- 类型: 字符串
- 默认值: default
- 环境变量: `FOUNDRY_REVERT_STRINGS` 或 `DAPP_REVERT_STRINGS`

可能的值包括：

- `default` 不注入编译器生成的回退字符串，保留用户提供的字符串。
- `strip` 删除所有回退字符串（如果可能，即如果使用字面量），保留副作用。
- `debug` 为编译器生成的内部回退注入字符串，目前为 ABI 编码器 V1 和 V2 实现。
- `verboseDebug` 甚至将更多信息附加到用户提供的回退字符串（尚未实现）。

##### `extra_output_files`

- 类型: 字符串数组
- 默认值: 无
- 环境变量: 无

应写入工件目录的 Solidity 编译器额外输出。

有效值包括：

- `metadata`: 写入工件目录中的 `metadata.json` 文件
- `ir`: 写入工件目录中的 `.ir` 文件
- `irOptimized`: 写入工件目录中的 `.iropt` 文件
- `ewasm`: 写入工件目录中的 `.ewasm` 文件
- `evm.assembly`: 写入工件目录中的 `.asm` 文件

##### `extra_output`

- 类型: 字符串数组
- 默认值: 见下文
- 环境变量: 无

在合约工件中包含的额外输出。

以下值总是设置，因为它们是 Forge 所需的：

```toml
extra_output = [
  "abi",
  "evm.bytecode",
  "evm.deployedBytecode",
  "evm.methodIdentifiers",
]
```

有关有效值的列表，参见 [Solidity 文档][output-desc]。

##### `bytecode_hash`

- 类型: 字符串
- 默认值: ipfs
- 环境变量: `FOUNDRY_BYTECODE_HASH` 或 `DAPP_BYTECODE_HASH`

确定用于字节码附加元数据哈希的方法。

有效值包括：

- ipfs (默认)
- bzzr1
- none

##### `sparse_mode`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_SPARSE_MODE` 或 `DAPP_SPARSE_MODE`

启用 [稀疏模式](../forge/forge-build.md#sparse-mode-experimental) 进行构建。

### 优化器

与 Solidity 优化器相关的配置。

##### `optimizer`

- 类型: 布尔值
- 默认值: true
- 环境变量: `FOUNDRY_OPTIMIZER` 或 `DAPP_OPTIMIZER`

是否启用 Solidity 优化器。

##### `optimizer_runs`

- 类型: 整数
- 默认值: 200
- 环境变量: `FOUNDRY_OPTIMIZER_RUNS` 或 `DAPP_OPTIMIZER_RUNS`

执行的优化器运行次数。

##### `via_ir`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_VIA_IR` 或 `DAPP_VIA_IR`

如果设置为 true，将编译管道更改为通过新的 IR 优化器。

##### `use_literal_content`

- 类型: 布尔值
- 默认值: false

如果设置为 true，编译将仅使用字面量内容，不使用 URL。

##### `[optimizer_details]`

优化器细节部分用于调整 Solidity 优化器的行为。此部分有几个可配置的值（每个都是布尔值）：

- `peephole`
- `inliner`
- `jumpdestRemover`
- `orderLiterals`
- `deduplicate`
- `cse`
- `constantOptimizer`
- `yul`

参见 Solidity [编译器输入描述](https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-input-and-output-json-description) 获取默认值。

##### `[optimizer_details.yul_details]`

优化器细节部分的 Yul 细节子部分用于调整新的 IR 优化器的行为。有两个配置值：

- `stack_allocation`: 尝试通过更早释放它们来改善堆栈槽的分配。
- `optimizer_steps`: 选择要应用的优化器步骤。

参见 Solidity [编译器输入描述](https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-input-and-output-json-description) 获取默认值。

> ℹ️ **注意**
> 如果在使用 `via_ir` 时遇到编译器错误，请显式启用旧版 `optimizer` 并将 `optimizer_steps` 留空

### 模型检查器

Solidity 模型检查器是 Solidity 编译器中内置的可选模块，适用于 OSX 和 Linux。在 [Solidity 编译器文档](https://docs.soliditylang.org/en/latest/smtchecker.html#tutorial) 中了解更多关于模型检查器的信息

> ℹ️ **注意**
> 模型检查器需要 `z3` 版本 4.8.8 或 4.8.14 在 Linux 上。

模型检查器设置在配置的 `[model_checker]` 部分中配置。

模型检查器将在调用 `forge build` 时运行，任何发现将显示为警告。

以下是使用模型检查器时的推荐设置：

```toml
[profile.default.model_checker]
contracts = {'/path/to/project/src/Contract.sol' = ['Contract']}
engine = 'chc'
timeout = 10000
targets = ['assert']
```

设置要验证的合约非常重要，否则所有可用的合约都将被验证，这可能需要很长时间。

推荐的引擎是 `chc`，但 `bmc` 和 `all`（运行两者）也是接受的。

设置适当的超时时间（以毫秒为单位）也很重要，因为默认时间可能不足以给底层求解器。

如果没有给出验证目标，则仅检查断言。

##### `[model_checker]`

模型检查器部分中可用的键如下。

###### `model_checker.contracts`

- 类型: 表
- 默认值: 所有
- 环境变量: 无

指定模型检查器将分析的合约。

表的键是源文件的路径，值是要检查的合约名称数组。

例如：

```toml
[profile.default.model_checker]
contracts = { "src/MyContracts.sol" = ["ContractA", "ContractB"] }
```

###### `model_checker.div_mod_with_slacks`

- 类型: 布尔值
- 默认值: false
- 环境变量: 无

设置如何编码除法和取模操作。

参见 [Solidity 文档](https://docs.soliditylang.org/en/latest/smtchecker.html#division-and-modulo-with-slack-variables) 获取更多信息。

###### `model_checker.engine`

- 类型: 字符串（见下文）
- 默认值: all
- 环境变量: 无

指定要运行的模型检查器引擎。有效值包括：

- `chc`: 约束 Horn 子句引擎
- `bmc`: 有界模型检查引擎
- `all`: 运行两个引擎

参见 [Solidity 文档](https://docs.soliditylang.org/en/latest/smtchecker.html#model-checking-engines) 获取更多关于引擎的信息。

###### `model_checker.invariants`

- 类型: 字符串数组
- 默认值: 无
- 环境变量: 无

设置模型检查器不变量。有效值包括：

- `contract`: 合约不变量
- `reentrancy`: 重入属性

参见 [Solidity 文档](https://docs.soliditylang.org/en/latest/smtchecker.html#reported-inferred-inductive-invariants) 获取更多关于不变量的信息。

###### `model_checker.show_unproved`

- 类型: 布尔值
- 默认值: false
- 环境变量: 无

是否输出所有未证明的目标。

参见 [Solidity 文档](https://docs.soliditylang.org/en/latest/smtchecker.html#unproved-targets) 获取更多信息。

###### `model_checker.solvers`

- 类型: 字符串数组
- 默认值: 无
- 环境变量: 无

设置模型检查器求解器。有效值包括：

- `cvc4`
- `eld`: 在 v0.8.18 中引入
- `smtlib2`
- `z3`

参见 [Solidity 文档](https://docs.soliditylang.org/en/latest/smtchecker.html#smt-and-horn-solvers) 获取更多信息。

###### `model_checker.timeout`

- 类型: 数字 (毫秒)
- 默认值: 无
- 环境变量: 无

设置底层模型检查器引擎的超时时间（以毫秒为单位）。

###### `model_checker.targets`

- 类型: 字符串数组
- 默认值: assert
- 环境变量: 无

设置模型检查器目标。有效值包括：

- `assert`: 断言
- `underflow`: 算术下溢
- `overflow`: 算术上溢
- `divByZero`: 除以零
- `constantCondition`: 平凡条件和不可达代码
- `popEmptyArray`: 弹出空数组
- `outOfBounds`: 数组/固定字节索引访问越界
- `default`: 上述所有（注意：不是 Forge 的默认值）
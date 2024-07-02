## forge build

### NAME

forge-build - 构建项目的智能合约。

### SYNOPSIS

``forge build`` 或 ``forge b`` [*options*]

### DESCRIPTION

构建项目的智能合约。

该命令将尝试通过查看所有合约和依赖项的版本要求来检测可以编译项目的最新版本。

你可以通过传递 `--no-auto-detect` 来覆盖此行为。或者，你可以传递 `--use <SOLC_VERSION>`。

如果该命令检测到它正在使用的 Solidity 编译器版本未安装，它将下载并安装在 `~/.svm` 中。你可以通过传递 `--offline` 来禁用此行为。

构建是增量的，构建缓存默认保存在项目根目录的 `cache/` 中。如果你想清除缓存，传递 `--force`，如果你想更改缓存目录，传递 `--cache-path <PATH>`。

可以通过指定多个路径选项（可以是源目录或文件的路径）来选择要构建的源。

#### 构建模式

有三种构建模式：

- 仅编译（默认）：构建项目并将合约工件保存在 `out/` 中（或由 `--out <PATH>` 指定的路径）。
- 大小模式（`--sizes`）：构建项目，显示非测试合约的大小，如果任何合约超过大小限制，则退出并返回代码 1。
- 名称模式（`--names`）：构建项目，显示合约名称并退出。

#### 优化器

你可以通过传递 `--optimize` 来启用优化器，并且可以通过传递 `--optimizer-runs <RUNS>` 来调整优化器运行的次数。

你还可以通过传递 `--via-ir` 选择加入 Solidity IR 编译管道。在 [Solidity 文档][ir-pipeline] 中阅读有关 IR 管道的更多信息。

默认情况下，优化器是启用的，运行 200 个周期。

##### 条件优化器使用

许多项目使用 solc 优化器，无论是通过标准编译管道还是 IR 管道。但在某些情况下，优化器可能会显著降低编译速度。

一个使用优化器的项目配置文件可能看起来像这样用于常规编译：

```toml
[profile.default]
solc-version = "0.8.17"
optimizer = true
optimizer-runs = 10_000_000
```

或者像这样用于 `via-ir`：

```toml
[profile.default]
solc-version = "0.8.17"
via_ir = true
```

为了在开发和测试期间减少编译速度，一种方法是有一个 `lite` 配置文件，该配置文件关闭优化器，并在开发/测试周期中使用。更新后的常规编译配置文件可能看起来像这样：

```toml
[profile.default]
solc-version = "0.8.17"
optimizer = true
optimizer-runs = 10_000_000

[profile.lite]
optimizer = false
```

或者像这样用于 `via-ir`：

```toml
[profile.default]
solc-version = "0.8.17"
via_ir = true

[profile.lite.optimizer_details.yulDetails]
optimizerSteps = ''
```

设置好后，`forge build`（或 `forge test` / `forge script`）仍然使用标准配置文件，因此默认情况下，`forge script` 调用将使用生产设置部署你的合约。运行 `FOUNDRY_PROFILE=lite forge build`（以及相同的测试和脚本命令）将使用 lite 配置文件来减少编译时间。

> 你可以配置额外的优化器细节，请参见下面的 [Additional Optimizer Settings](#additional-optimizer-settings) 部分了解更多信息。

#### 工件

你可以通过传递 `--extra-output <SELECTOR>` 将 Solidity 编译器的额外输出添加到你的工件中。

选择器是 Solidity 编译器输出中的路径，你可以在 [Solidity 文档][output-desc] 中阅读更多相关信息。

你还可以通过传递 `--extra-output-files <SELECTOR>` 将一些编译器输出写入单独的文件。

`--extra-output-files` 的有效选择器是：

- `metadata`：作为 `metadata.json` 文件写入工件目录
- `ir`：作为 `.ir` 文件写入工件目录
- `irOptimized`：作为 `.iropt` 文件写入工件目录
- `ewasm`：作为 `.ewasm` 文件写入工件目录
- `evm.assembly`：作为 `.asm` 文件写入工件目录

#### 监视模式

可以通过传递 `--watch [PATH...]` 以监视模式运行该命令，每当监视的文件或目录发生变化时，它将重新构建。默认情况下会监视源目录。

#### 稀疏模式（实验性）

稀疏模式仅编译符合某些标准的文件。

默认情况下，此过滤器适用于自上次构建以来未更改的文件，但对于接受文件过滤器的命令（例如 [forge test](./forge-test.md)），稀疏模式将仅重新编译与过滤器匹配的文件。

稀疏模式是可选的，直到该功能稳定。要选择加入稀疏模式并试用，请在你的配置文件中设置 [`sparse_mode`][sparse-mode]。

#### 额外的优化器设置

优化器可以通过额外的设置进行微调。只需在你的配置文件中设置 `optimizer_details` 表。例如：

```toml
[profile.default.optimizer_details]
constantOptimizer = true
yul = true

[profile.default.optimizer_details.yulDetails]
stackAllocation = true
optimizerSteps = 'dhfoDgvulfnTUtnIf'
```

有关可用设置的更多信息，请参见 [编译器输入描述文档](https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-input-and-output-json-description)（特别是 `settings.optimizer.details`）。

#### 回退字符串

你可以通过编译器控制回退字符串的生成方式。默认情况下，只有用户提供的回退字符串包含在字节码中，但还有其他选项：

- `strip`：删除所有回退字符串（如果可能，即如果使用字面量），保持副作用。
- `debug`：注入编译器生成的内部回退字符串，目前为 ABI 编码器 V1 和 V2 实现。
- `verboseDebug`：将更多信息附加到用户提供的回退字符串（尚未实现）。

#### 额外的模型检查器设置

[Solidity 内置的模型检查器](https://docs.soliditylang.org/en/latest/smtchecker.html#tutorial) 是一个可选模块，可以通过 `ModelChecker` 对象启用。

参见 [编译器输入描述 `settings.modelChecker`](https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-input-and-output-json-description) 和 [模型检查器的选项](https://docs.soliditylang.org/en/latest/smtchecker.html#smtchecker-options-and-tuning)。

该模块在 `solc` 发布二进制文件中适用于 OSX 和 Linux。后者需要系统中安装 z3 库版本 [4.8.8, 4.8.14]（SO 版本 4.8）。

与上述优化器设置类似，`model_checker` 设置必须以前缀为它们对应的配置文件：`[profile.default.model_checker]` 属于 `[profile.default]`。

```toml
## foundry.toml
[profile.default.model_checker]
contracts = { '/path/to/project/src/Contract.sol' = [ 'Contract' ] }
engine = 'chc'
timeout = 10000
targets = [ 'assert' ]
```

以上字段在使用模型检查器时是推荐的。
设置要验证的合约非常重要，否则将验证所有可用的合约，这可能会消耗大量时间。
推荐的引擎是 `chc`，但也接受 `bmc` 和 `all`（运行两者）。
设置适当的超时时间（以毫秒为单位）也很重要，因为给底层求解器的时间可能不够。
如果没有给出验证目标，则只会检查断言。

调用 `forge build` 时，模型检查器将运行，如果有任何发现，将显示为警告。

### OPTIONS

#### 构建选项

`--names`
&nbsp;&nbsp;&nbsp;&nbsp;打印编译的合约名称。

`--sizes`
&nbsp;&nbsp;&nbsp;&nbsp;打印编译的非测试合约大小，如果任何合约超过大小限制，则退出并返回代码 1。

`--skip`
&nbsp;&nbsp;&nbsp;&nbsp;跳过编译非必要的合约目录，如测试或脚本（用法 `--skip test`）。

`[PATHS]...`&nbsp;&nbsp;&nbsp;&nbsp;从指定路径构建源文件。

{{#include core-build-options.md}}

{{#include watch-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 构建项目：
    ```sh
    forge build
    ```

2. 使用 solc 0.6.0 构建项目：
    ```sh
    forge build --use solc:0.6.0
    ```

3. 使用额外的工件输出构建项目：
    ```sh
    forge build --extra-output evm.assembly
    ```

4. 以监视模式构建项目：
    ```sh
    forge build --watch
    ```

5. 从 `test/invariant` 目录和 `test/RegressionTest.sol` 构建源文件：
    ```sh
    forge build test/invariant test/RegressionTest.sol
    ```

### SEE ALSO

[forge](./forge.md), [forge clean](./forge-clean.md), [forge inspect](./forge-inspect.md)

[sparse-mode]: ../config/solidity-compiler.md#sparse_mode
[ir-pipeline]: https://docs.soliditylang.org/en/latest/ir-breaking-changes.html
[output-desc]: https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-api
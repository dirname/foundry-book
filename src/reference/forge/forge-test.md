## forge test

### 名称

forge-test - 运行项目的测试。

### 概要

``forge test`` [*选项*]

### 描述

运行项目的测试。

#### 分叉

可以通过传递 `--fork-url <URL>` 在分叉环境中运行测试。

当测试在分叉环境中运行时，你可以访问分叉链的所有状态，就像你部署了合约一样。[作弊码][cheatcodes] 仍然可用。

你还可以通过传递 `--fork-block-number <BLOCK>` 指定从某个区块分叉。当从特定区块分叉时，链数据会缓存到 `~/.foundry/cache`。如果你不想缓存链数据，可以传递 `--no-storage-caching`。

在分叉环境中运行时，无法由本地合约解码的跟踪（例如对主网上存在的合约的调用，如代币）可以选择使用 Etherscan 解码。要使用 Etherscan 进行跟踪解码，请设置 `ETHERSCAN_API_KEY` 或传递 `--etherscan-api-key <KEY>`。

#### 调试

可以在交互式调试器中运行测试。要启动调试器，请传递 `--debug <TEST>`。

如果多个测试匹配指定的模式，你必须使用其他测试过滤器以将匹配的测试数量减少到恰好 1。

如果测试是单元测试，它会立即在调试器中打开。

如果测试是模糊测试，模糊测试会运行，调试器会在第一个失败的场景中打开。如果没有失败的场景，调试器会在最后一个场景中打开。

有关调试器的更多信息，请参见 [调试器章节][debugger]。

#### 气体报告

可以通过传递 `--gas-report` 生成气体报告。

有关气体报告的更多信息，请参见 [气体报告章节][gas-reports]。

#### 列表

可以列出测试而不运行它们。
你可以传递 `--json` 以便外部扩展更容易解析结构化内容。

### 选项

{{#include test-options.md}}

{{#include evm-options.md}}

{{#include executor-options.md}}

{{#include core-build-options.md}}

{{#include watch-options.md}}

{{#include ../common/display-options.md}}

`--list`  
&nbsp;&nbsp;&nbsp;&nbsp;列出测试而不是运行它们。

{{#include common-options.md}}

### 示例

1. 运行测试：
    ```sh
    forge test
    ```

2. 在调试器中打开测试：
    ```sh
    forge test --debug testSomething
    ```

3. 生成气体报告：
    ```sh
    forge test --gas-report
    ```

4. 仅在 `test/Contract.t.sol` 中的 `BigTest` 合约中运行以 `testFail` 开头的测试：
    ```sh
    forge test --match-path test/Contract.t.sol --match-contract BigTest \
      --match-test "testFail*"
    ```

5. 以所需格式列出测试
    ```sh
    forge test --list
    forge test --list --json
    forge test --list --json --match-test "testFail*" | tail -n 1 | json_pp
    ```

### 另请参阅

[forge](./forge.md), [forge build](./forge-build.md), [forge snapshot](./forge-snapshot.md)

[debugger]: ../../forge/debugger.md
[cheatcodes]: ../../cheatcodes/
[gas-reports]: ../../forge/gas-reports.md
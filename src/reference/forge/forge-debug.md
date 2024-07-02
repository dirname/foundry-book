## forge debug

### NAME

forge-debug - 将单个智能合约作为脚本进行调试。

### SYNOPSIS

``forge debug`` [*options*] *path* [*args...*]

### DESCRIPTION

将位于源文件（*path*）中的单个智能合约作为脚本进行调试。

如果指定的源文件中有多个合约，则必须传递 `--target-contract` 来指定要运行的合约。

#### 调用

在脚本部署到内部 EVM 后，如果存在，会调用一个带有 `setUp()` 签名的函数。

默认情况下，假定脚本包含在一个带有 `run()` 签名的函数中。如果希望运行不同的函数，请传递 `--sig <SIGNATURE>`。

签名可以是片段（`<function name>(<types>)`），或原始调用数据。

如果传递片段，并且函数有参数，可以将调用参数添加到命令末尾（*args*）。

#### 分叉

可以通过传递 `--fork-url <URL>` 在分叉环境中运行脚本。

当脚本在分叉环境中运行时，可以访问分叉链的所有状态，就像部署了脚本一样。[作弊码][cheatcodes] 仍然可用。

还可以通过传递 `--fork-block-number <BLOCK>` 指定从某个区块分叉。当从特定区块分叉时，链数据会缓存到 `~/.foundry/cache`。如果不希望缓存链数据，请传递 `--no-storage-caching`。

#### 调试

可以在交互式调试器中运行脚本。要启动调试器，请传递 `--debug`。

有关调试器的更多信息，请参见 [调试器章节][debugger]。

### OPTIONS

#### 调试选项

`--target-contract` *contract_name*  
&nbsp;&nbsp;&nbsp;&nbsp;要运行的合约名称

`-s` *signature*  
`--sig` *signature*  
&nbsp;&nbsp;&nbsp;&nbsp;要在合约中调用的函数签名或原始调用数据。默认值：`run()`

`--debug`  
&nbsp;&nbsp;&nbsp;&nbsp;在 [调试器][debugger] 中打开脚本。

{{#include evm-options.md}}

{{#include executor-options.md}}

{{#include core-build-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 执行合约中的 `run()` 函数：
    ```sh
    forge debug src/Contract.sol
    ```

2. 在调试器中打开脚本：
    ```sh
    forge debug src/Contract.sol --debug
    ```

3. 执行合约中的 `foo()` 函数：
    ```sh
    forge debug src/Contract.sol --sig "foo()"
    ```

4. 执行带有参数的合约函数：
    ```sh
    forge debug src/Contract.sol --sig "foo(string,uint256)" "hello" 100
    ```

5. 使用原始调用数据执行合约：
    ```sh
    forge debug src/Contract.sol --sig "0x..."
    ```

### SEE ALSO

[forge](./forge.md), [forge test](./forge-test.md)

[debugger]: ../../forge/debugger.md
[cheatcodes]: ../../cheatcodes/
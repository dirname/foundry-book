## cast run

### NAME

cast-run - 在本地环境中运行已发布的交易并打印追踪信息。

### SYNOPSIS

``cast run`` [*options*] `--rpc-url` *url* *tx_hash*

### DESCRIPTION

在本地环境中运行已发布的交易并打印追踪信息。

默认情况下，在您想要重放的交易之前的区块中的所有交易也会被重放。如果您想要更快的结果，可以使用 `--quick`，但是结果可能与实际执行不同。

您还可以通过传递 `--debug` 在调试器中打开交易。

### OPTIONS

#### 运行选项

`--label` *label*  
&nbsp;&nbsp;&nbsp;&nbsp;在追踪信息中标记一个地址。  
&nbsp;&nbsp;&nbsp;&nbsp;格式为 `<address>:<label>`。可以多次传递。

`-q`  
`--quick`  
&nbsp;&nbsp;&nbsp;&nbsp;仅使用前一个区块的状态执行交易。  
&nbsp;&nbsp;&nbsp;&nbsp;可能导致与实际执行不同的结果！

`-v`  
`--verbose`  
&nbsp;&nbsp;&nbsp;&nbsp;地址完全显示，而不是被截断。

`-d`  
`--debug`  
&nbsp;&nbsp;&nbsp;&nbsp;在 [调试器][debugger] 中打开脚本。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 重放一个交易（一个简单的转账）：
    ```sh
    cast run 0xd15e0237413d7b824b784e1bbc3926e52f4726e5e5af30418803b8b327b4f8ca
    ```

2. 重放一个交易，应用在前一个区块的状态之上：
    ```sh
    cast run --quick \
      0xd15e0237413d7b824b784e1bbc3926e52f4726e5e5af30418803b8b327b4f8ca
    ```

3. 重放一个交易并标记地址：
    ```sh
    cast run \
      --label 0xc564ee9f21ed8a2d8e7e76c085740d5e4c5fafbe:sender \
      --label 0x40950267d12e979ad42974be5ac9a7e452f9505e:recipient \
      --label 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2:weth \
      0xd15e0237413d7b824b784e1bbc3926e52f4726e5e5af30418803b8b327b4f8ca
    ```

4. 在调试器中重放一个交易：
    ```sh
    cast run --debug \
      0xd15e0237413d7b824b784e1bbc3926e52f4726e5e5af30418803b8b327b4f8ca
    ```

### SEE ALSO

[cast](./cast.md)

[debugger]: ../../forge/debugger.md
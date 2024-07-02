## cast receipt

### NAME

cast-receipt - 获取交易的收据。

### SYNOPSIS

``cast receipt`` [*options*] *tx_hash* [*field*]

### DESCRIPTION

获取交易的收据。

如果指定了 *field*，则仅显示收据的指定字段。

### OPTIONS

#### Receipt Options

`--async`  
`--cast-async`  
&nbsp;&nbsp;&nbsp;&nbsp;如果收据尚不存在，则不等待交易收据。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量: `CAST_ASYNC`

`-c` *confirmations*  
`--confirmations` *confirmations*  
&nbsp;&nbsp;&nbsp;&nbsp;在退出前等待一定数量的确认。默认值: `1`。

#### RPC Options

{{#include ../common/rpc-url-option.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取交易收据:
    ```sh
    cast receipt $TX_HASH
    ```

2. 获取交易被包含的区块号:
    ```sh
    cast receipt $TX_HASH blockNumber
    ```

### SEE ALSO

[cast](./cast.md), [cast call](./cast-call.md), [cast send](./cast-send.md), [cast publish](./cast-publish.md)
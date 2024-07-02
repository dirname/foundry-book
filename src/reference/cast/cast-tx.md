## cast tx

### NAME

cast-tx - 获取交易信息。

### SYNOPSIS

``cast tx`` [*options*] *tx_hash* [*field*]

### DESCRIPTION

获取交易信息。

如果指定了 *field*，则仅显示交易的指定字段。

### OPTIONS

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取交易信息：
    ```sh
    cast tx $TX_HASH
    ```

2. 获取交易的 sender：
    ```sh
    cast tx $TX_HASH from
    ```

### SEE ALSO

[cast](./cast.md), [cast receipt](./cast-receipt.md)
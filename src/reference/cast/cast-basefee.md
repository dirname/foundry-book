## cast basefee

### NAME

cast-base-fee - 获取区块的基本费用。

### SYNOPSIS

``cast base-fee`` [*options*] [*block*]

### DESCRIPTION

获取区块的基本费用。

指定的 *block* 可以是区块号，或者是以下标签之一：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。默认为 `latest`。

### OPTIONS

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取最新区块的基本费用：
    ```sh
    cast base-fee
    ```

2. 获取创世区块的基本费用：
    ```sh
    cast base-fee 1
    ```

### SEE ALSO

[cast](./cast.md), [cast block](./cast-block.md), [cast age](./cast-age.md)
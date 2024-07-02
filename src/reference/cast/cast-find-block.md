## cast find-block

### NAME

cast-find-block - 获取最接近提供时间戳的区块号。

### SYNOPSIS

``cast find-block`` [*options*] *timestamp*

### DESCRIPTION

获取最接近提供时间戳的区块号。

### OPTIONS

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取最接近2021年新年的区块号
    ```sh
    cast find-block 1609459200
    ```

### SEE ALSO

[cast](./cast.md)
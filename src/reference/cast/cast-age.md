## cast age

### NAME

cast-age - 获取区块的时间戳。

### SYNOPSIS

``cast age`` [*options*] [*block*]

### DESCRIPTION

获取区块的时间戳。

指定的 *block* 可以是区块号，或者是以下标签之一：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。默认为 `latest`。

### OPTIONS

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取最新区块的时间戳：
    ```sh
    cast age
    ```

2. 获取创世区块的时间戳：
    ```sh
    cast age 1
    ```

### SEE ALSO

[cast](./cast.md), [cast block](./cast-block.md), [cast basefee](./cast-basefee.md)
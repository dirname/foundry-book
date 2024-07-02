## cast nonce

### NAME

cast-nonce - 获取账户的nonce值。

### SYNOPSIS

``cast nonce`` [*options*] *who*

### DESCRIPTION

获取账户的nonce值。

参数 *who* 可以是ENS名称或地址。

### OPTIONS

#### 查询选项

`-B` *block*  
`--block` *block*  
&nbsp;&nbsp;&nbsp;&nbsp;你想查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;可以是区块号，或以下任一标签：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。

#### RPC选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 beer.eth 的nonce值
    ```sh
    cast nonce beer.eth
    ```

### SEE ALSO

[cast](./cast.md), [cast balance](./cast-balance.md)
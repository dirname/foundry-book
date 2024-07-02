## cast code

### NAME

cast-code - 获取合约的字节码。

### SYNOPSIS

``cast code`` [*options*] *address*

### DESCRIPTION

获取合约的字节码。

合约（*address*）可以是 ENS 名称或地址。

### OPTIONS

#### 查询选项

`-B` *block*  
`--block` *block*  
&nbsp;&nbsp;&nbsp;&nbsp;你想查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;可以是区块号，或以下任一标签：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 WETH 合约的字节码。
    ```sh
    cast code 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

### SEE ALSO

[cast](./cast.md), [cast proof](./cast-proof.md)
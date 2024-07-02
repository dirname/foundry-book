## cast codesize

### NAME

cast-codesize - 获取合约的运行时代码大小。

### SYNOPSIS

``cast codesize`` [*options*] *address*

### DESCRIPTION

获取合约的运行时代码大小。

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

1. 获取 WETH 合约的运行时代码大小。
```sh
cast codesize 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
```

### SEE ALSO

[cast](./cast.md), [cast code](./cast-code.md)
## cast access-list

### NAME

cast-access-list - 创建一个交易访问列表。

### SYNOPSIS

``cast access-list`` [*options*] *to* *sig* [*args...*]

### DESCRIPTION

创建一个交易访问列表。

目标地址（*to*）可以是 ENS 名称或地址。

{{#include sig-description.md}}

### OPTIONS

#### 查询选项

`-B` *block*  
`--block` *block*  
&nbsp;&nbsp;&nbsp;&nbsp;你想要查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;可以是区块号，或以下任何标签：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。

{{#include ../common/wallet-options.md}}

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

{{#include common-options.md}}

### SEE ALSO

[cast](./cast.md), [cast send](./cast-send.md), [cast publish](./cast-publish.md), [cast call](./cast-call.md)
## cast compute-address

### NAME

cast-compute-address - 根据给定的nonce和部署者地址计算合约地址。

### SYNOPSIS

``cast compute-address`` [*options*] *address*

### DESCRIPTION

根据给定的nonce和部署者地址计算合约地址。

### OPTIONS

#### 计算选项

`--nonce` *nonce*  
&nbsp;&nbsp;&nbsp;&nbsp;账户的nonce。默认为从RPC获取的最新nonce。

#### RPC选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### SEE ALSO

[cast](./cast.md), [cast proof](./cast-proof.md), [cast create2](./cast-create2.md)
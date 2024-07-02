## cast resolve-name

### NAME

cast-resolve-name - 执行 ENS 查询。

### SYNOPSIS

``cast resolve-name`` [*options*] *who*

### DESCRIPTION

执行 ENS 查询。

如果传递 `--verify`，则在正常查询之后执行反向查询以验证名称是否正确。

### OPTIONS

#### 查询选项

`-v`  
`--verify`  
&nbsp;&nbsp;&nbsp;&nbsp;执行反向查询以验证名称是否正确。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 ENS 名称的地址。
    ```sh
    cast resolve-name vitalik.eth
    ```

2. 执行正常查询和反向查询：
    ```sh
    cast resolve-name --verify vitalik.eth
    ```

### SEE ALSO

[cast](./cast.md), [cast lookup-address](./cast-lookup-address.md)
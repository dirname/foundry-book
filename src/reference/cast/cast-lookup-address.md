## cast lookup-address

### NAME

cast-lookup-address - 执行 ENS 反向查找。

### SYNOPSIS

``cast lookup-address`` [*options*] *who*

### DESCRIPTION

执行 ENS 反向查找。

如果传递 `--verify`，则在反向查找之后执行正常查找以验证地址是否正确。

### OPTIONS

#### 查找选项

`-v`  
`--verify`  
&nbsp;&nbsp;&nbsp;&nbsp;执行正常查找以验证地址是否正确。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取地址的 ENS 名称。
    ```sh
    cast lookup-address $ADDRESS
    ```

2. 执行反向查找和正常查找：
    ```sh
    cast lookup-address --verify $ADDRESS
    ```

### SEE ALSO

[cast](./cast.md), [cast resolve-name](./cast-resolve-name.md)
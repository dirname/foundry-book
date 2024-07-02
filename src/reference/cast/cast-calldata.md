## cast calldata

### NAME

cast-calldata - ABI 编码带有参数的函数。

### SYNOPSIS

``cast calldata`` [*options*] *sig* [*args...*]

### DESCRIPTION

ABI 编码带有参数的函数。

签名 (*sig*) 是一个形式为 `<function name>(<types...>)` 的片段。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. ABI 编码调用 `someFunc(address,uint256)` 的参数：
    ```sh
    cast calldata "someFunc(address,uint256)" 0x... 1
    ```

### SEE ALSO

[cast](./cast.md), [cast abi-encode](./cast-abi-encode.md)
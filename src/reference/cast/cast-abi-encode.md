## cast abi-encode

### NAME

cast-abi-encode - ABI 编码给定的函数参数，不包括选择器。

### SYNOPSIS

``cast abi-encode`` [*options*] *sig* [*args...*]

### DESCRIPTION

ABI 编码给定的函数，不包括选择器。

签名 (*sig*) 是一个形式为 `<function name>(<types...>)` 的片段。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. ABI 编码调用 `someFunc(address,uint256)` 的参数：
    ```sh
    cast abi-encode "someFunc(address,uint256)" 0x... 1
    ```

2. 对于编码包含组件的类型（如元组或自定义结构体）：

    ```sh
    cast abi-encode "someFunc((string,uint256))" "(myString,1)"
    ```

### SEE ALSO

[cast](./cast.md), [cast calldata](./cast-calldata.md)
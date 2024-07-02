## cast calldata-decode

### NAME

cast-calldata-decode - 解码 ABI 编码的输入数据。

### SYNOPSIS

``cast calldata-decode`` [*options*] *sig* *calldata*

### DESCRIPTION

解码 ABI 编码的输入数据。

签名（*sig*）是一个形式为 `<function name>(<types...>)` 的片段。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 解码 `transfer` 调用的输入数据：
    ```sh
    cast calldata-decode "transfer(address,uint256)" \
      0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d0000000000000000000000000000000000000000000000000008a8e4b1a3d8000
    ```

### SEE ALSO

[cast](./cast.md), [cast abi-decode](./cast-abi-decode.md)
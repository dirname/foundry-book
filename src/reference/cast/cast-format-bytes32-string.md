## cast format-bytes32-string

### NAME

cast-format-bytes32-string - 将字符串格式化为 bytes32 编码。

### SYNOPSIS

``cast format-bytes32-string`` [*options*] *string*

### DESCRIPTION

将字符串格式化为 bytes32 编码。

请注意，此命令仅用于将 [Solidity 字符串字面量](https://docs.soliditylang.org/en/v0.8.16/types.html#string-literals-and-types) 格式化为 `bytes32`。如果您需要填充字节字符串，请使用 [to-bytes32](./cast-to-bytes32.md)。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将字符串 "hello" 转换为 bytes32 十六进制：
    ```sh
    cast format-bytes32-string "hello"
    ```

### SEE ALSO

[cast](./cast.md)
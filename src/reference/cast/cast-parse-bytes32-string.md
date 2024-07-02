## cast parse-bytes32-string

### NAME

cast-parse-bytes32-string - 从 bytes32 编码解析字符串。

### SYNOPSIS

``cast parse-bytes32-string`` [*options*] *bytes*

### DESCRIPTION

从其 bytes32 编码表示中解析 [Solidity 字符串字面量](https://docs.soliditylang.org/en/v0.8.16/types.html#string-literals-and-types)，主要通过将字节解释为 ASCII 字符。此命令撤销 [--format-bytes32-string](./cast-format-bytes32-string.md) 中的编码。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将 "hello" 的 bytes32 字符串编码解析回字符串表示：
    ```sh
    cast parse-bytes32-string "0x68656c6c6f000000000000000000000000000000000000000000000000000000"
    ```

### SEE ALSO

[cast](./cast.md)
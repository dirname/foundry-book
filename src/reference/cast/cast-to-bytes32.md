## cast to-bytes32

### NAME

cast-to-bytes32 - 将十六进制数据右填充到32字节。

### SYNOPSIS

``cast to-bytes32`` [*options*] *bytes*

### DESCRIPTION

将十六进制数据右填充到32字节。

请注意，此命令仅用于填充字节字符串。如果您想将[Solidity字符串字面量](https://docs.soliditylang.org/en/v0.8.16/types.html#string-literals-and-types)格式化为`bytes32`，请使用[format-bytes32-string](./cast-format-bytes32-string.md)。

### OPTIONS

{{#include common-options.md}}

### SEE ALSO

[cast](./cast.md)
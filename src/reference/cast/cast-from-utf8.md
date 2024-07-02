## cast from-utf8

### NAME

cast-from-utf8 - 将 UTF8 文本转换为十六进制。

### SYNOPSIS

``cast from-utf8`` [*options*] *text*

### DESCRIPTION

将 UTF8 文本转换为十六进制。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将 UTF8 文本转换为十六进制：
    ```sh
    cast from-utf8 "hello"
    ```

### SEE ALSO

[cast](./cast.md)
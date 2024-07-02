## cast to-ascii

### NAME

cast-to-ascii - 将十六进制数据转换为 ASCII 字符串。

### SYNOPSIS

``cast to-ascii`` [*options*] *text*

### DESCRIPTION

将十六进制数据转换为 ASCII 字符串。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将十六进制数据转换为 ASCII 字符串：
    ```sh
    cast to-ascii "0x68656c6c6f"
    ```

### SEE ALSO

[cast](./cast.md)
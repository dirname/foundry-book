## cast to-dec

### NAME

cast-to-dec - 将一个基数的数字转换为十进制

### SYNOPSIS

``cast to-dec`` [*options*] *value*

### DESCRIPTION

将一个基数的数字转换为十进制

### OPTIONS

`--base-in` *base_in*
&nbsp;&nbsp;&nbsp;&nbsp;输入的基数。

{{#include common-options.md}}

### EXAMPLES

1. 将十六进制的 ff 转换为十进制
    ```sh
    cast to-dec ff
    ```

### SEE ALSO

[cast](./cast.md)
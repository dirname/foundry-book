## cast to-base

### NAME

cast-to-base - 将一个基数的数字转换为另一个基数。

### SYNOPSIS

``cast to-base`` [*options*] *value* *base*

### DESCRIPTION

将一个基数的数字转换为另一个基数。

### OPTIONS

#### Base Options

`--base-in` *base*
&nbsp;&nbsp;&nbsp;&nbsp;输入数字的基数。可用选项：

&nbsp;&nbsp;&nbsp;&nbsp;10, d, dec, decimal

&nbsp;&nbsp;&nbsp;&nbsp;16, h, hex, hexadecimal

{{#include common-options.md}}

### EXAMPLES

1. 将十进制数 64 转换为十六进制
    ```sh
    cast to-base 64 hex
    ```

2. 将十六进制数 100 转换为二进制
    ```sh
    cast to-base 0x100 2
    ```

> 注意：--base-in 参数不是强制的，但如果输入不明确则需要使用。

### SEE ALSO

[cast](./cast.md)
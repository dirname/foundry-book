## cast shr

### NAME

cast-shr - 执行右移操作。

### SYNOPSIS

``cast shr`` [*options*] *value* *shift*

### DESCRIPTION

执行右移操作。

### OPTIONS

{{#include ../common/base-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 对 0x12 执行一次右移位操作
    ```sh
    cast shr --base-in 16 0x12 1
    ```

> 注意：--base-in 参数不是强制的，但如果输入不明确，则需要使用。

### SEE ALSO

[cast](./cast.md), [cast shl](./cast-shl.md)
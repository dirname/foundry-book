## cast shl

### NAME

cast-shl - 执行左移操作。

### SYNOPSIS

``cast shl`` [*options*] *value* *shift*

### DESCRIPTION

执行左移操作。

### OPTIONS

{{#include ../common/base-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 对数字 61 执行 3 位左移
    ```sh
    cast shl --base-in 10 61 3
    ```

> 注意：--base-in 参数不是强制的，但如果输入不明确则需要使用。

### SEE ALSO

[cast](./cast.md), [cast shr](./cast-shr.md)
## cast to-fixed-point

### NAME

cast-to-fixed-point - 将整数转换为定点数。

### SYNOPSIS

``cast to-fixed-point`` [*options*] *decimals* *value*

### DESCRIPTION

将整数转换为定点数。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将250转换为带有2位小数的定点数：
    ```sh
    cast to-fixed-point 2 250
    ```

### SEE ALSO

[cast](./cast.md)
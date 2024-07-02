## cast from-fixed-point

### NAME

cast-from-fixed-point - 将定点数转换为整数。

### SYNOPSIS

``cast from-fixed-point`` [*options*] *decimals* *value*

### DESCRIPTION

将定点数转换为整数。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将 10.55 转换为整数：
    ```sh
    cast from-fixed-point 2 10.55
    ```

### SEE ALSO

[cast](./cast.md)
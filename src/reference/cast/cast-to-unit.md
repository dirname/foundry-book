## cast to-unit

### NAME

cast-to-unit - 将以太币数量转换为另一种单位。

### SYNOPSIS

``cast to-unit`` [*options*] *value* [*unit*]

### DESCRIPTION

将以太币数量转换为另一种单位。

要转换的值（*value*）可以是以太币的数量（以 wei 为单位），或者是带有单位的数字。

有效的单位包括：

- `ether`
- `gwei`
- `wei`

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将 1000 wei 转换为 gwei
    ```sh
    cast to-unit 1000 gwei
    ```

2. 将 1 eth 转换为 gwei
    ```sh
    cast to-unit 1ether gwei
    ```

### SEE ALSO

[cast](./cast.md)
## cast 4byte

### NAME

cast-4byte - 从 <https://sig.eth.samczsun.com> 获取给定选择器的函数签名。

### SYNOPSIS

``cast 4byte`` [*options*] *sig*

### DESCRIPTION

从 <https://sig.eth.samczsun.com> 获取给定选择器的函数签名。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 获取选择器 `0x8cc5ce99` 的函数签名：
    ```sh
    cast 4byte 0x8cc5ce99
    ```

### SEE ALSO

[cast](./cast.md), [cast 4byte-decode](./cast-4byte-decode.md), [cast 4byte-event](./cast-4byte-event.md), [cast selectors](./cast-selectors.md)
## cast sig-event

### NAME

cast-sig-event - 从事件字符串生成事件签名。

### SYNOPSIS

``cast sig-event`` [*options*] *event_string*

### DESCRIPTION

从事件字符串生成事件签名。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 获取日志 `Transfer(address indexed from, address indexed to, uint256 amount)` 的哈希：
    ```sh
    cast sig-event "Transfer(address indexed from, address indexed to, uint256 amount)"
    ```

### SEE ALSO

[cast](./cast.md)
## cast 4byte-event

### NAME

cast-4byte-event - 从 <https://sig.eth.samczsun.com> 获取给定 topic 0 的事件签名。

### SYNOPSIS

``cast 4byte-event`` [*options*] *topic_0*

### DESCRIPTION

从 <https://sig.eth.samczsun.com> 获取给定 topic 0 的事件签名。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 获取 topic 0 为 `0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` 的事件签名：
    ```sh
    cast 4byte-event 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
    ```

### SEE ALSO

[cast](./cast.md), [cast 4byte](./cast-4byte.md), [cast 4byte-decode](./cast-4byte-decode.md)
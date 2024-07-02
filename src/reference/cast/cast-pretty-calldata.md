## cast pretty-calldata

### NAME

cast-pretty-calldata - 美化打印 calldata。

### SYNOPSIS

``cast pretty-calldata`` [*options*] *calldata*

### DESCRIPTION

美化打印 calldata。

除非传递 `--offline`，否则会尝试使用 <https://sig.eth.samczsun.com> 解码 calldata。

### OPTIONS

#### 4byte 选项

`-o`  
`--offline`  
&nbsp;&nbsp;&nbsp;&nbsp;跳过 <https://sig.eth.samczsun.com> 的查找。

{{#include common-options.md}}

### EXAMPLES

1. 解码 `transfer` 调用的 calldata：
    ```sh
    cast pretty-calldata 0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d00000000000000000000000000000000000000000000000000174b37380cea000
    ```

### SEE ALSO

[cast](./cast.md), [cast 4byte-decode](./cast-4byte-decode.md)
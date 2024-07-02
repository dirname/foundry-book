## cast 4byte-decode

### NAME

cast-4byte-decode - 使用 <https://sig.eth.samczsun.com> 解码 ABI 编码的调用数据。

### SYNOPSIS

``cast 4byte-decode`` [*options*] *calldata*

### DESCRIPTION

使用 <https://sig.eth.samczsun.com> 解码 ABI 编码的调用数据。

### OPTIONS

#### 4byte 选项

`--id` *id*  
&nbsp;&nbsp;&nbsp;&nbsp;要使用的解析签名的索引。
&nbsp;&nbsp;&nbsp;&nbsp;  
&nbsp;&nbsp;&nbsp;&nbsp;<https://sig.eth.samczsun.com> 可以为给定的选择器提供多个可能的签名。  
&nbsp;&nbsp;&nbsp;&nbsp;索引可以是整数，或者是标签 "earliest" 和 "latest"。

{{#include common-options.md}}

### EXAMPLES

1. 解码 `transfer` 调用的调用数据：
    ```sh
    cast 4byte-decode 0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d00000000000000000000000000000000000000000000000000174b37380cea000
    ```

### SEE ALSO

[cast](./cast.md), [cast 4byte](./cast-4byte.md), [cast 4byte-event](./cast-4byte-event.md)
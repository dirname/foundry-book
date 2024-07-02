## cast from-rlp

### NAME

cast-from-rlp - 解码RLP编码的数据。

### SYNOPSIS

``cast from-rlp`` *data*

### DESCRIPTION

解码RLP编码的数据。

*data* 是一个带有可选0x前缀的十六进制字符串。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 解码RLP数据：
    ```sh
    cast from-rlp 0xc481f181f2

    cast from-rlp c481f181f2
    ```
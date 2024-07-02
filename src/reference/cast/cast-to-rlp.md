## cast to-rlp

### NAME

cast-to-rlp - 将十六进制数据编码为RLP。

### SYNOPSIS

``cast to-rlp`` *array*

### DESCRIPTION

将十六进制字符串或十六进制字符串的JSON数组进行RLP编码。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 编码RLP数据：
    ```sh
    cast to-rlp '["0xaa","0xbb","cc"]'
   
    cast to-rlp f0a9     
    ```
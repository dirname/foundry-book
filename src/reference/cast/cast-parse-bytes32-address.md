## cast parse-bytes32-address

### NAME

cast-parse-bytes32-address - 从 bytes32 编码中解析出校验和地址。

### SYNOPSIS

``cast parse-bytes32-address`` [*options*] *bytes*

### DESCRIPTION

从其 bytes32 编码表示中解析出校验和地址。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 将 WETH9 合约地址的 bytes32 编码解析为其地址表示：
    ```sh
    cast parse-bytes32-address 0x000000000000000000000000C02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

### SEE ALSO

[cast](./cast.md)
## cast wallet address

### NAME

cast-wallet-address - 将私钥转换为地址。

### SYNOPSIS

``cast wallet address`` [*options*]

### DESCRIPTION

将私钥转换为地址。

### OPTIONS

#### Keystore 选项

{{#include ../common/wallet-options-raw.md}}

{{#include ../common/wallet-options-keystore.md}}

{{#include ../common/wallet-options-hardware.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 `keystore.json` 中的密钥对的地址：
    ```sh
    cast wallet address --keystore keystore.json
    ```

### SEE ALSO

[cast](./cast.md), [cast wallet](./cast-wallet.md)
## cast wallet sign

### NAME

cast-wallet-sign - 签名消息。

### SYNOPSIS

``cast wallet sign`` [*options*] *message*

### DESCRIPTION

签名消息。

### OPTIONS

{{#include ../common/wallet-options-raw.md}}

{{#include ../common/wallet-options-keystore.md}}

{{#include ../common/wallet-options-hardware.md}}

{{#include common-options.md}}

### EXAMPLES

1. 使用 keystore 签名消息：
    ```sh
    cast wallet sign --keystore keystore.json --interactive "hello"
    ```

2. 使用原始私钥签名消息：
    ```sh
    cast wallet sign --private-key $PRIV_KEY "hello"
    ```

### SEE ALSO

[cast](./cast.md), [cast wallet](./cast-wallet.md)
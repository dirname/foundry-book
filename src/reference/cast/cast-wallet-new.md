## cast wallet new

### NAME

cast-wallet-new - 创建一个新的随机密钥对。

### SYNOPSIS

``cast wallet new`` [*options*] [*path*]

### DESCRIPTION

创建一个新的随机密钥对。

如果指定了 *path*，则新的密钥对将写入一个使用密码加密的 JSON 密钥库。
（*path* 应为一个现有目录。）

### OPTIONS

#### Keystore Options

`-p`  
`--password`  
&nbsp;&nbsp;&nbsp;&nbsp;触发 JSON 密钥库的隐藏密码提示。  
&nbsp;&nbsp;&nbsp;&nbsp;**已弃用：现在默认提示隐藏密码。**

`--unsafe-password` *password*  
&nbsp;&nbsp;&nbsp;&nbsp;JSON 密钥库的明文密码。

&nbsp;&nbsp;&nbsp;&nbsp;这**不安全**，建议使用 `--password` 代替。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量：`CAST_PASSWORD`

{{#include common-options.md}}

### EXAMPLES

1. 创建一个新的密钥对，但不保存到密钥库：
    ```sh
    cast wallet new
    ```

2. 创建一个新的密钥对并保存到 `keystore` 目录：
    ```sh
    cast wallet new keystore
    ```

### SEE ALSO

[cast](./cast.md), [cast wallet](./cast-wallet.md)
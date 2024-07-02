## cast wallet vanity

### 名称

cast-wallet-vanity - 生成一个自定义地址。

### 概要

``cast wallet vanity`` [*选项*]

### 描述

生成一个自定义地址。

如果指定了 `--nonce`，则该命令将尝试生成一个自定义合约地址。`--save-path` 选项允许指定一个自定义文件路径来保存生成的钱包详细信息。

### 选项

#### 密钥库选项

`--starts-with` *hex*  
&nbsp;&nbsp;&nbsp;&nbsp;自定义地址的前缀。

`--ends-with` *hex*  
&nbsp;&nbsp;&nbsp;&nbsp;自定义地址的后缀。

`--nonce` *nonce*  
&nbsp;&nbsp;&nbsp;&nbsp;生成一个由生成的密钥对创建的自定义合约地址，并指定 nonce。

`--save-path` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;保存生成的自定义钱包的路径。如果提供，钱包详细信息将保存在此位置的 JSON 文件中。

{{#include common-options.md}}

### 示例

1. 创建一个以 `dead` 开头的新密钥对：
    ```sh
    cast wallet vanity --starts-with dead
    ```

2. 创建一个以 `beef` 结尾的新密钥对：
    ```sh
    cast wallet vanity --ends-with beef
    ```

3. 创建一个以 `dead` 开头的新密钥对，并将详细信息保存到特定路径：
    ```sh
    cast wallet vanity --starts-with dead --save-path /path/to/save
    ```

### 另请参阅

[cast](./cast.md), [cast wallet](./cast-wallet.md)
## cast publish

### NAME

cast-publish - 将原始交易发布到网络。

### SYNOPSIS

``cast publish`` [*options*] *tx*

### DESCRIPTION

将预先签名的原始交易发布到网络。

### OPTIONS

#### Publish Options

`--async`  
`--cast-async`  
&nbsp;&nbsp;&nbsp;&nbsp;不等待交易收据。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量: `CAST_ASYNC`

{{#include ../common/rpc-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 发布预签名的交易：
    ```sh
    cast publish --rpc-url $RPC $TX
    ```

2. 使用 flashbots 发布预签名的交易：
    ```sh
    cast publish --flashbots $TX
    ```

### SEE ALSO

[cast](./cast.md), [cast call](./cast-call.md), [cast send](./cast-send.md), [cast receipt](./cast-receipt.md), [cast mktx](./cast-mktx.md)
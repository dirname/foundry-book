## cast balance

### NAME

cast-balance - 获取账户余额（以 wei 为单位）。

### SYNOPSIS

``cast balance`` [*options*] *who*

### DESCRIPTION

获取账户余额。

参数 *who* 可以是 ENS 名称或地址。

### OPTIONS

#### 查询选项

`-B` *block*  
`--block` *block*  
&nbsp;&nbsp;&nbsp;&nbsp;你想查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;可以是区块号，或任何标签：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。

`-e` *ether*  
`--ether` *ether*  
&nbsp;&nbsp;&nbsp;&nbsp;如果使用此标志，则余额将以 ether 显示。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 beer.eth 的余额
    ```sh
    cast balance beer.eth
    ```
2. 使用 RPC URL 获取任何地址的 ERC20 余额
    ```sh
    # 加载 .env 文件中的变量
    source .env

    # 获取 Binance 的 USDT 余额
    cast balance --erc20 0xdAC17F958D2ee523a2206206994597C13D831ec7 0xF977814e90dA44bFA03b6295A0616a897441aceC --rpc-url $MAINNET_RPC_URL
    ```

### SEE ALSO

[cast](./cast.md), [cast nonce](./cast-nonce.md)
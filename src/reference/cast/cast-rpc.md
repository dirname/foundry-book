## cast rpc

### NAME

cast-rpc - 执行原始 JSON-RPC 请求

### 概要

``cast rpc`` [*选项*] *方法* [*参数...*]

### 描述

对给定的方法和参数执行简单的 JSON-RPC POST 请求。

### 选项

#### 查询选项

`-r` *url*  
`--rpc-url` *url*  
&nbsp;&nbsp;&nbsp;&nbsp;提供者的 URL

`-w`  
`--raw`  
&nbsp;&nbsp;&nbsp;&nbsp;按原样传递“参数”
&nbsp;&nbsp;&nbsp;&nbsp;如果传递了 --raw，第一个参数将被视为“参数”的值。如果没有给出参数，将使用标准输入。例如：
&nbsp;&nbsp;&nbsp;&nbsp; rpc eth_getBlockByNumber '["0x123", false]' --raw
&nbsp;&nbsp;&nbsp;&nbsp;   => {"method": "eth_getBlockByNumber", "params": ["0x123", false] ... }

### 示例

1. 在本地获取最新的 `eth_getBlockByNumber`：

    ```sh
    cast rpc eth_getBlockByNumber "latest" "false"
    ```

2. 在本地获取 `eth_getTransactionByHash`：

    ```sh
    cast rpc eth_getTransactionByHash 0x2642e960d3150244e298d52b5b0f024782253e6d0b2c9a01dd4858f7b4665a3f
    ```
    
3. 在以太坊主网获取最新的 `eth_getBlockByNumber`：

   ```sh
   cast rpc --rpc-url https://mainnet.infura.io/v3/ eth_getBlockByNumber "latest" "false"
   ```
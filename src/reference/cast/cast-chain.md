## cast chain

### 名称

cast-chain - 获取当前链的符号名称。

### 概要

``cast chain`` [*选项*]

### 描述

从我们连接的 RPC 端点获取符号链名称。

### 选项

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### 示例

1. 在与 `$RPC` 通信时获取链名称：
    ```sh
    cast chain --rpc-url $RPC
    ```

2. 当 `$ETH_RPC_URL` 已设置时获取链名称：
    ```sh
    cast chain
    ```

### 另请参阅

[cast](./cast.md), [cast chain-id](./cast-chain-id.md)
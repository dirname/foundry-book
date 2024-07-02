## cast chain-id

### NAME

cast-chain-id - 获取以太坊链 ID。

### SYNOPSIS

``cast chain-id`` [*options*]

### DESCRIPTION

从我们连接的 RPC 端点获取以太坊 [链 ID][chain-id]。

### OPTIONS

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include common-options.md}}

### EXAMPLES

1. 在与 `$RPC` 通信时获取链 ID：
    ```sh
    cast chain-id --rpc-url $RPC
    ```

2. 当 `$ETH_RPC_URL` 已设置时获取链 ID：
    ```sh
    cast chain-id
    ```

### SEE ALSO

[cast](./cast.md), [cast chain](./cast-chain.md)

[chain-id]: https://chainlist.org/
## cast proof

### NAME

cast-proof - 生成给定存储槽的存储证明。

### SYNOPSIS

``cast proof`` [*options*] *address* [*slots...*]

### DESCRIPTION

生成给定存储槽的存储证明。

地址（*address*）可以是 ENS 名称或地址。

显示的输出是一个 JSON 对象，包含以下键：

- `accountProof`：账户本身的证明
- `address`：账户地址
- `balance`：账户余额
- `codeHash`：账户代码的哈希
- `nonce`：账户的 nonce
- `storageHash`：账户存储的哈希
- `storageProof`：存储证明的数组，每个请求的槽一个
- `storageProof.key`：槽
- `storageProof.proof`：槽的证明
- `storageProof.value`：槽的值

### OPTIONS

#### 查询选项

`-B` *block*  
`--block` *block*  
&nbsp;&nbsp;&nbsp;&nbsp;你想要查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;可以是区块号，或任何标签：`earliest`、`finalized`、`safe`、`latest` 或 `pending`。

#### RPC 选项

{{#include ../common/rpc-url-option.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取 WETH 合约存储槽 0 的证明：
    ```sh
    cast proof 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 0
    ```

### SEE ALSO

[cast](./cast.md), [cast storage](./cast-storage.md)
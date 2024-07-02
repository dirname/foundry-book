## cast logs

### NAME

cast logs - 通过签名或主题获取日志。

### SYNOPSIS

``cast logs`` [*options*] *sig_or_topic* [*topics_or_args...*]


### DESCRIPTION

通过签名或主题获取日志。

(*sig_or_topic*) 可以是事件签名或其哈希主题（位于 topics[0]）。

如果使用签名，剩余参数必须是普通形式。如果使用主题，参数必须与它们自身作为主题的形式一致。

### OPTIONS

### 查询选项

`--from-block` *from_block*
&nbsp;&nbsp;&nbsp;&nbsp;开始查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;也可以是以下标签：`earliest`, `finalized`, `safe`, `latest`, 或 `pending`。

`--to-block` *to_block*
&nbsp;&nbsp;&nbsp;&nbsp;停止查询的区块高度。

&nbsp;&nbsp;&nbsp;&nbsp;也可以是以下标签：`earliest`, `finalized`, `safe`, `latest`, 或 `pending`。

`--address` *address*
&nbsp;&nbsp;&nbsp;&nbsp;过滤的合约地址

{{#include ../common/wallet-options.md}}

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

### EXAMPLES

1. 使用签名获取日志：
    ```sh
    cast logs --from-block 15537393 --to-block latest 'Transfer (address indexed from, address indexed to, uint256 value)' 0x2e8ABfE042886E4938201101A63730D04F160A82
    ```
2. 使用主题获取日志：
    ```sh
    cast logs --from-block 15537393 --to-block latest 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef 0x0000000000000000000000002e8abfe042886e4938201101a63730d04f160a82
    ```

### SEE ALSO

[cast](./cast.md)
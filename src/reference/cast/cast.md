## cast

### NAME

cast - 从命令行舒适地执行以太坊RPC调用。

### SYNOPSIS

`cast` [*options*] *command* [*args*]
`cast` [*options*] `--version`
`cast` [*options*] `--help`

### DESCRIPTION

这个程序是一组工具，用于与以太坊交互并执行转换。

### COMMANDS

#### General Commands

[cast help](./cast-help.md)
&nbsp;&nbsp;&nbsp;&nbsp;显示关于Cast的帮助信息。

[cast completions](./cast-completions.md)
&nbsp;&nbsp;&nbsp;&nbsp;为Cast生成Shell自动补全。

#### Chain Commands

[cast chain-id](./cast-chain-id.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取以太坊链ID。

[cast chain](./cast-chain.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取当前链的符号名称。

[cast client](./cast-client.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取当前客户端版本。

#### Transaction Commands

[cast publish](./cast-publish.md)
&nbsp;&nbsp;&nbsp;&nbsp;将原始交易发布到网络。

[cast receipt](./cast-receipt.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取交易的收据。

[cast send](./cast-send.md)
&nbsp;&nbsp;&nbsp;&nbsp;签名并发布交易。

[cast call](./cast-call.md)
&nbsp;&nbsp;&nbsp;&nbsp;在不发布交易的情况下对账户进行调用。

[cast rpc](./cast-rpc.md)
&nbsp;&nbsp;&nbsp;&nbsp;执行原始JSON-RPC请求 [aliases: rp]

[cast tx](./cast-tx.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取交易信息。

[cast run](./cast-run.md)
&nbsp;&nbsp;&nbsp;&nbsp;在本地环境中运行已发布的交易并打印追踪信息。

[cast estimate](./cast-estimate.md)
&nbsp;&nbsp;&nbsp;&nbsp;估算交易的 gas 成本。

[cast access-list](./cast-access-list.md)
&nbsp;&nbsp;&nbsp;&nbsp;为交易创建访问列表。

[cast logs](./cast-logs.md)
&nbsp;&nbsp;&nbsp;&nbsp;通过签名或主题获取日志。

#### Block Commands

[cast find-block](./cast-find-block.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取最接近提供时间戳的区块号。

[cast gas-price](./cast-gas-price.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取当前 gas 价格。

[cast block-number](./cast-block-number.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取最新区块号。

[cast basefee](./cast-basefee.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取区块的 basefee。

[cast block](./cast-block.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取区块信息。

[cast age](./cast-age.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取区块的时间戳。

#### Account Commands

[cast balance](./cast-balance.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取账户的余额（以 wei 为单位）。

[cast storage](./cast-storage.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取合约存储槽的原始值。

[cast proof](./cast-proof.md)
&nbsp;&nbsp;&nbsp;&nbsp;为给定的存储槽生成存储证明。

[cast nonce](./cast-nonce.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取账户的 nonce。

[cast code](./cast-code.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取合约的字节码。

[cast codesize](./cast-codesize.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取合约的运行时字节码大小。

#### ENS Commands

[cast lookup-address](./cast-lookup-address.md)
&nbsp;&nbsp;&nbsp;&nbsp;执行 ENS 反向查找。

[cast resolve-name](./cast-resolve-name.md)
&nbsp;&nbsp;&nbsp;&nbsp;执行 ENS 查找。

[cast namehash](./cast-namehash.md)
&nbsp;&nbsp;&nbsp;&nbsp;计算 ENS 名称的 namehash。

#### Etherscan Commands

[cast etherscan-source](./cast-etherscan-source.md)
&nbsp;&nbsp;&nbsp;&nbsp;从 Etherscan 获取合约的源代码。

#### ABI Commands

[cast abi-decode](./cast-abi-decode.md)
&nbsp;&nbsp;&nbsp;&nbsp;解码 ABI 编码的输入或输出数据。

[cast abi-encode](./cast-abi-encode.md)
&nbsp;&nbsp;&nbsp;&nbsp;ABI 编码给定的函数参数，不包括选择器。

[cast 4byte](./cast-4byte.md)
&nbsp;&nbsp;&nbsp;&nbsp;从 <https://sig.eth.samczsun.com> 获取给定选择器的函数签名。

[cast 4byte-decode](./cast-4byte-decode.md)
&nbsp;&nbsp;&nbsp;&nbsp;使用 <https://sig.eth.samczsun.com> 解码 ABI 编码的 calldata。

[cast 4byte-event](./cast-4byte-event.md)
&nbsp;&nbsp;&nbsp;&nbsp;从 <https://sig.eth.samczsun.com> 获取给定 topic 0 的事件签名。

[cast calldata](./cast-calldata.md)
&nbsp;&nbsp;&nbsp;&nbsp;ABI 编码带有参数的函数。

[cast calldata-decode](./cast-calldata-decode.md)
&nbsp;&nbsp;&nbsp;&nbsp;解码 ABI 编码的输入数据。

[cast pretty-calldata](./cast-pretty-calldata.md)
&nbsp;&nbsp;&nbsp;&nbsp;美化打印 calldata。

[cast selectors](./cast-selectors.md)
&nbsp;&nbsp;&nbsp;&nbsp;从字节码中提取函数选择器和参数。

[cast upload-signature](./cast-upload-signature.md)
&nbsp;&nbsp;&nbsp;&nbsp;将给定的签名上传到 https://sig.eth.samczsun.com。

#### Conversion Commands

[cast format-bytes32-string](./cast-format-bytes32-string.md)
&nbsp;&nbsp;&nbsp;&nbsp;将字符串格式化为 bytes32 编码。

[cast from-bin](./cast-from-bin.md)
&nbsp;&nbsp;&nbsp;&nbsp;将二进制数据转换为十六进制数据。

[cast from-fixed-point](./cast-from-fixed-point.md)
&nbsp;&nbsp;&nbsp;&nbsp;将定点数转换为整数。

[cast from-utf8](./cast-from-utf8.md)
&nbsp;&nbsp;&nbsp;&nbsp;将 UTF8 转换为十六进制。

[cast from-wei](./cast-from-wei.md)
&nbsp;&nbsp;&nbsp;&nbsp;将 wei 转换为 ETH 金额。

[cast parse-bytes32-address](./cast-parse-bytes32-address.md)
&nbsp;&nbsp;&nbsp;&nbsp;从 bytes32 编码解析校验和地址。

[cast parse-bytes32-string](./cast-parse-bytes32-string.md)
&nbsp;&nbsp;&nbsp;&nbsp;从 bytes32 编码解析字符串。

[cast to-ascii](./cast-to-ascii.md)
&nbsp;&nbsp;&nbsp;&nbsp;将十六进制数据转换为 ASCII 字符串。

[cast to-base](./cast-to-base.md)
&nbsp;&nbsp;&nbsp;&nbsp;将一个基数的数转换为另一个基数。

[cast to-bytes32](./cast-to-bytes32.md)
&nbsp;&nbsp;&nbsp;&nbsp;将十六进制数据右填充到 32 字节。

[cast to-dec](./cast-to-dec.md)
&nbsp;&nbsp;&nbsp;&nbsp;将一个基数的数转换为十进制。

[cast to-fixed-point](./cast-to-fixed-point.md)
&nbsp;&nbsp;&nbsp;&nbsp;将整数转换为定点数。

[cast to-hex](./cast-to-hex.md)
&nbsp;&nbsp;&nbsp;&nbsp;将一个基数的数转换为另一个基数。

[cast to-hexdata](./cast-to-hexdata.md)
&nbsp;&nbsp;&nbsp;&nbsp;将输入规范化为小写、0x 前缀的十六进制。

[cast to-int256](./cast-to-int256.md)
&nbsp;&nbsp;&nbsp;&nbsp;将一个数转换为十六进制编码的 int256。

[cast to-rlp](./cast-to-rlp.md)
&nbsp;&nbsp;&nbsp;&nbsp;RLP 编码十六进制数据，或十六进制数据的数组。

[cast to-uint256](./cast-to-uint256.md)
&nbsp;&nbsp;&nbsp;&nbsp;将一个数转换为十六进制编码的 uint256。

[cast to-unit](./cast-to-unit.md)
&nbsp;&nbsp;&nbsp;&nbsp;将 ETH 金额转换为另一个单位（ether、gwei、wei）。

[cast to-wei](./cast-to-wei.md)
&nbsp;&nbsp;&nbsp;&nbsp;将 ETH 金额转换为 wei。

[cast shl](./cast-shl.md)
&nbsp;&nbsp;&nbsp;&nbsp;执行左移操作。

[cast shr](./cast-shr.md)
&nbsp;&nbsp;&nbsp;&nbsp;执行右移操作。

#### Utility Commands

[cast address-zero](./cast-address-zero.md)
&nbsp;&nbsp;&nbsp;&nbsp;打印零地址。

[cast sig](./cast-sig.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取函数的 selector。

[cast sig-event](./cast-sig-event.md)
&nbsp;&nbsp;&nbsp;&nbsp;从事件字符串生成事件签名。

[cast keccak](./cast-keccak.md)
&nbsp;&nbsp;&nbsp;&nbsp;使用 keccak-256 哈希任意数据。

[cast compute-address](./cast-compute-address.md)
&nbsp;&nbsp;&nbsp;&nbsp;根据给定的 nonce 和部署者地址计算合约地址。

[cast create2](./cast-create2.md)
&nbsp;&nbsp;&nbsp;&nbsp;使用 CREATE2 生成确定性合约地址。

[cast interface](./cast-interface.md)
&nbsp;&nbsp;&nbsp;&nbsp;根据给定的 ABI 生成 Solidity 接口。

[cast index](./cast-index.md)
&nbsp;&nbsp;&nbsp;&nbsp;计算映射中条目的存储槽。

[cast concat-hex](./cast-concat-hex.md)
&nbsp;&nbsp;&nbsp;&nbsp;连接十六进制字符串。

[cast max-int](./cast-max-int.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取最大 i256 值。

[cast min-int](./cast-min-int.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取最小 i256 值。

[cast max-uint](./cast-max-uint.md)
&nbsp;&nbsp;&nbsp;&nbsp;获取最大 u256 值。

[cast to-check-sum-address](./cast-to-check-sum-address.md)
&nbsp;&nbsp;&nbsp;&nbsp;将地址转换为校验和格式（EIP-55）。

#### Wallet Commands

[cast wallet](./cast-wallet.md)
&nbsp;&nbsp;&nbsp;&nbsp;钱包管理工具。

[cast wallet new](./cast-wallet-new.md)
&nbsp;&nbsp;&nbsp;&nbsp;创建一个新的随机密钥对。

[cast wallet address](./cast-wallet-address.md)
&nbsp;&nbsp;&nbsp;&nbsp;将私钥转换为地址。

[cast wallet sign](./cast-wallet-sign.md)
&nbsp;&nbsp;&nbsp;&nbsp;签名消息。

[cast wallet vanity](./cast-wallet-vanity.md)
&nbsp;&nbsp;&nbsp;&nbsp;生成 vanity 地址。

[cast wallet verify](./cast-wallet-verify.md)
&nbsp;&nbsp;&nbsp;&nbsp;验证消息的签名。

### OPTIONS

#### Special Options

`-V`
`--version`
&nbsp;&nbsp;&nbsp;&nbsp;打印版本信息并退出。

{{#include common-options.md}}

### EXAMPLES

1. 调用合约上的函数：

    ```sh
    cast call 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 \
      "balanceOf(address)(uint256)" 0x...
    ```

2. 解码原始 calldata：

    ```sh
    cast calldata-decode "transfer(address,uint256)" \
      0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d0000000000000000000000000000000000000000000000000008a8e4b1a3d8000
    ```

3. 编码 calldata：
    ```sh
    cast calldata "someFunc(address,uint256)" 0x... 1
    ```

### BUGS

请参见 <https://github.com/foundry-rs/foundry/issues> 获取问题列表。
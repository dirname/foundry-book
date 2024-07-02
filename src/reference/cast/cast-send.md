## cast send

### NAME

cast-send - 签名并发布交易。

### SYNOPSIS

``cast send`` [*options*] *to* [*sig*] [*args...*]

### DESCRIPTION

签名并发布交易。

目标地址（*to*）可以是 ENS 名称或地址。

{{#include sig-description.md}}

### OPTIONS

{{#include ../common/transaction-options.md}}

`--resend`  
&nbsp;&nbsp;&nbsp;&nbsp;重用发送账户的最新 nonce。

`--create` *code* [*sig* *args...*]  
&nbsp;&nbsp;&nbsp;&nbsp;通过指定原始字节码来部署合约，而不是指定 *to* 地址。

#### Receipt Options

`--async`  
`--cast-async`  
&nbsp;&nbsp;&nbsp;&nbsp;如果交易收据尚不存在，则不等待交易收据。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量: `CAST_ASYNC`

`-c` *confirmations*  
`--confirmations` *confirmations*  
&nbsp;&nbsp;&nbsp;&nbsp;在退出前等待一定数量的确认。默认: `1`。

{{#include ../common/wallet-options.md}}

`--unlocked`  
&nbsp;&nbsp;&nbsp;&nbsp;通过 `eth_sendTransaction` 使用 `--from` 参数或 `$ETH_FROM` 作为发送者。

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 使用 Ledger 向 Vitalik 发送一些以太币：
    ```sh
    cast send --ledger vitalik.eth --value 0.1ether
    ```

2. 在合约上调用 `deposit(address token, uint256 amount)` 函数：
    ```sh
    cast send --ledger 0x... "deposit(address,uint256)" 0x... 1
    ```

3. 调用一个期望 `struct` 的函数：

    ```solidity
    contract Test {
        struct MyStruct {
            address addr;
            uint256 amount;
        }
        function myfunction(MyStruct memory t) public pure {}
    }
    ```

    结构体被编码为元组（参见 [struct encoding](../../misc/struct-encoding.md)）

    ```sh
    cast send 0x... "myfunction((address,uint256))" "(0x...,1)"
    ```

4. 在交易的 `input` 字段中发送带有十六进制数据的交易：
    ```sh
    cast send 0x... 0x68656c6c6f20776f726c64
    ```

### SEE ALSO

[cast](./cast.md), [cast call](./cast-call.md), [cast publish](./cast-publish.md), [cast receipt](./cast-receipt.md), [cast mktx](./cast-mktx.md), [struct encoding](../../misc/struct-encoding.md)
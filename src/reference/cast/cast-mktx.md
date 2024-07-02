## cast mktx

### 名称

cast-mktx - 构建并签署交易。

### 概要

``cast mktx`` [*选项*] *to* [*sig*] [*args...*]

### 描述

构建并签署交易，但不发布它。

目标地址（*to*）可以是 ENS 名称或地址。

{{#include sig-description.md}}

### 选项

{{#include ../common/transaction-options.md}}

`--create` *code* [*sig* *args...*]  
&nbsp;&nbsp;&nbsp;&nbsp;通过指定原始字节码而不是指定 *to* 地址来部署合约。

{{#include ../common/wallet-options-raw.md}}

{{#include ../common/wallet-options-keystore.md}}

{{#include ../common/wallet-options-hardware.md}}

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

{{#include common-options.md}}

### 示例

1. 使用 Ledger 签署向 Vitalik 发送一些以太币的交易：
    ```sh
    cast mktx --ledger vitalik.eth --value 0.1ether
    ```

2. 签署调用合约上 `deposit(address token, uint256 amount)` 函数的交易：
    ```sh
    cast mktx --ledger 0x... "deposit(address,uint256)" 0x... 1
    ```

3. 签署调用期望 `struct` 的函数的交易：

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
    cast mktx 0x... "myfunction((address,uint256))" "(0x...,1)"
    ```

4. 在交易的 `input` 字段中使用十六进制数据签署交易：
    ```sh
    cast mktx 0x... 0x68656c6c6f20776f726c64
    ```

### 参见

[cast](./cast.md), [cast publish](./cast-publish.md), [cast send](./cast-send.md), [struct encoding](../../misc/struct-encoding.md)
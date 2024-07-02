## cast interface

### NAME

cast-interface - 从给定的 ABI 生成 Solidity 接口。

### SYNOPSIS

``cast interface`` [*options*] *address_or_path*

### DESCRIPTION

从给定的 ABI 生成 Solidity 接口。

参数 (*address_or_path*) 可以是包含 ABI 的文件路径，或者是地址。

如果提供地址，则从 Etherscan 获取账户的 ABI 生成接口。

> ℹ️ **注意**
>
> 该命令目前不支持 ABI 编码器 v2。

### OPTIONS

#### Interface Options

`-n` *name*  
`--name` *name*  
&nbsp;&nbsp;&nbsp;&nbsp;用于生成接口的名称。默认名称为 `Interface`。

`-o` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;输出文件的路径。如果未指定，接口将输出到标准输出。

`-p` *version*  
`--pragma` *version*  
&nbsp;&nbsp;&nbsp;&nbsp;用于接口的 Solidity pragma 版本。默认: `^0.8.10`。

`-j`  
`--json`  
&nbsp;&nbsp;&nbsp;&nbsp;输出合约的 JSON ABI。

{{#include ../common/etherscan-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 从文件生成接口：
    ```sh
    cast interface ./path/to/abi.json
    ```

2. 使用 Etherscan 生成接口：
    ```sh
    cast interface -o IWETH.sol 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

3. 从文件生成并命名接口：
    ```sh
    cast interface -n LilENS ./path/to/abi.json
    ```

4. 从 Etherscan 获取合约的 JSON ABI：
    ```sh
    cast interface -o IWETH.sol -j 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

### SEE ALSO

[cast](./cast.md), [cast proof](./cast-proof.md)
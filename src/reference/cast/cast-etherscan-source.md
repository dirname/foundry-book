## cast etherscan-source

### NAME

cast-etherscan-source - 从Etherscan获取合约的源代码。

### SYNOPSIS

``cast etherscan-source`` [*options*] *address*

### DESCRIPTION

从Etherscan获取合约的源代码。

目标地址（*to*）可以是ENS名称或地址。

### OPTIONS

#### 输出选项

`-d` *directory*  
&nbsp;&nbsp;&nbsp;&nbsp;将源代码树展开到的输出目录。
&nbsp;&nbsp;&nbsp;&nbsp;如果未提供，源代码将输出到标准输出。

{{#include ../common/etherscan-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 获取WETH合约的源代码：
    ```sh
    cast etherscan-source 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

2. 将WETH合约的源代码展开到一个名为`weth`的目录中：
    ```sh
    cast etherscan-source -d weth 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
    ```

### SEE ALSO

[cast](./cast.md)
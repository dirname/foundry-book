## cast selectors

### NAME

cast-selectors - 从字节码中提取函数选择器和参数

### SYNOPSIS

``cast selectors`` [*options*] *bytecode*

### DESCRIPTION

使用 [EVMole 库](https://github.com/cdump/evmole) 从字节码中提取函数选择器和参数

### OPTIONS

`-r`  
`--resolve`  
&nbsp;&nbsp;&nbsp;&nbsp;使用 https://openchain.xyz 解析提取的选择器的函数签名

{{#include common-options.md}}

### EXAMPLES

1. 获取 WETH 合约的函数签名和参数：
    ```sh
    cast selectors $(cast code 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2)
    ```

### SEE ALSO

[cast](./cast.md), [cast 4byte](./cast-4byte.md)
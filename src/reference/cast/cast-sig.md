## cast sig

### NAME

cast-sig - 获取函数的签名选择器。

### SYNOPSIS

``cast sig`` [*options*] *sig*

### DESCRIPTION

获取函数的签名选择器。

签名（*sig*）是一个形式为 `<function name>(<types...>)` 的片段。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 获取函数 `transfer(address,uint256)` 的选择器：
    ```sh
    cast sig "transfer(address,uint256)"
    ```

2. 获取一个期望 `struct` 的函数的选择器：

    ```solidity
    contract Test {
        struct MyStruct {
            address addr;
            uint256 amount;
        }
        function myfunction(MyStruct memory t) public pure {}
    }
    ```

    Structs 被编码为元组（参见 [struct 编码](../../misc/struct-encoding.md)）。

    ```sh
    cast sig "myfunction((address,uint256))"
    ```

### SEE ALSO

[cast](./cast.md), [struct 编码](../../misc/struct-encoding.md)
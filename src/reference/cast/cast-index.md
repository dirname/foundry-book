## cast index

### NAME

cast-index - 计算映射中条目的存储槽位置。

### SYNOPSIS

``cast index`` *key_type* *key* *slot*

### DESCRIPTION

计算映射中条目的存储槽位置。

使用 `cast storage` 获取值。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES
```solidity
// World.sol

mapping (address => uint256) public mapping1;
mapping (string => string) public mapping2;
```

1. 计算类型为 `mapping(string => string)` 的映射中条目 (`hello`) 的存储槽位置，位于槽 1：
    ```sh
    >> cast index string "hello" 1
    0x3556fc8e3c702d4479a1ab7928dd05d87508462a12f53307b5407c969223d1f8
    >> cast storage [address] 0x3556fc8e3c702d4479a1ab7928dd05d87508462a12f53307b5407c969223d1f8
    world
    ```

### SEE ALSO

[cast](./cast.md)
## forge flatten

### 名称

forge-flatten - 将一个源文件及其所有导入合并到一个文件中。

### 概要

``forge flatten`` [*选项*] *文件*

### 描述

将一个源文件及其所有导入合并到一个文件中。

如果未设置 `--output <FILE>`，则合并后的合约将输出到标准输出。

### 选项

#### 合并选项

`-o` *文件*  
`--output` *文件*  
&nbsp;&nbsp;&nbsp;&nbsp;输出合并后合约的路径。如果未指定，合并后的合约将输出到标准输出。

{{#include project-options.md}}

{{#include common-options.md}}

### 示例

1. 合并 `src/Contract.sol`：
    ```sh
    forge flatten src/Contract.sol
    ```

2. 合并 `src/Contract.sol` 并将结果写入 `src/Contract.flattened.sol`：
    ```sh
    forge flatten --output src/Contract.flattened.sol src/Contract.sol
    ```

### 另请参阅

[forge](./forge.md), [forge verify-contract](./forge-verify-contract.md)
## forge coverage

### 名称

forge-coverage - 显示代码中哪些部分被测试覆盖。

### 概要

`forge coverage` [*选项*]

### 描述

显示代码中哪些部分被测试覆盖。

### 选项

#### 报告选项

`--report` 允许你指定用于覆盖率的报告类型。此标志可以多次使用。

它有三种不同的选项，默认设置为 `summary`。

`summary`  
&nbsp;&nbsp;&nbsp;&nbsp;输出一个图表，显示你的代码被测试覆盖的百分比。

`lcov`  
&nbsp;&nbsp;&nbsp;&nbsp;在项目的根目录中创建一个包含覆盖数据的 lcov.info 文件。

`debug`  
&nbsp;&nbsp;&nbsp;&nbsp;输出描述未覆盖代码位置的行。

{{#include common-options.md}}

#### 优化选项

`--ir-minimum` 允许你使用 `via-ir` 启用以进行“最少量的优化”（[minimum amount of optimization](https://github.com/ethereum/solidity/issues/12533#issuecomment-1013073350)）。

### 示例

1. 查看汇总覆盖率：

   ```sh
   forge coverage
   ```

2. 创建包含覆盖数据的 lcov 文件：

   ```sh
   forge coverage --report lcov
   ```

3. 输出未覆盖代码的位置：
   ```sh
   forge coverage --report debug
   ```

### 另请参阅

[forge](./forge.md), [forge test](./forge-test.md)
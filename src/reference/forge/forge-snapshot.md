## forge snapshot

### NAME

forge-snapshot - 创建每个测试的 gas 使用情况快照。

### SYNOPSIS

``forge snapshot`` [*options*]

### DESCRIPTION

创建每个测试的 gas 使用情况快照。

结果会写入一个名为 `.gas-snapshot` 的文件。你可以通过传递 `--snap <PATH>` 来更改文件名。

默认情况下，快照中包含 fuzz 测试。它们使用静态种子以实现确定性结果。

可以使用 `--diff` 和 `--check` 来比较快照。第一个标志会输出差异，第二个标志会输出差异并如果快照不匹配则退出并返回代码 1。

### OPTIONS

#### Snapshot Options

`--asc`  
按 gas 使用量升序排序结果。

`--desc`  
&nbsp;&nbsp;&nbsp;&nbsp;按 gas 使用量降序排序结果。

`--min` *min_gas*  
&nbsp;&nbsp;&nbsp;&nbsp;仅包含使用 gas 量超过给定数量的测试。

`--max` *max_gas*  
&nbsp;&nbsp;&nbsp;&nbsp;仅包含使用 gas 量少于给定数量的测试。

`--tolerance` *threshold*  
&nbsp;&nbsp;&nbsp;&nbsp;容忍 gas 偏差高达指定百分比（0-100）。

`--diff` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;与预先存在的快照输出差异。

&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，比较使用 `.gas-snapshot`。

`--check` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;与预先存在的快照进行比较，如果不匹配则退出并返回代码 1。

&nbsp;&nbsp;&nbsp;&nbsp;如果快照不匹配，则输出差异。

&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，比较使用 `.gas-snapshot`。

`--snap` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;快照的输出文件。默认：`.gas-snapshot`。

{{#include test-options.md}}

{{#include evm-options.md}}

{{#include executor-options.md}}

{{#include core-build-options.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 创建快照：
    ```sh
    forge snapshot
    ```

2. 生成差异：
    ```sh
    forge snapshot --diff
    ```

3. 检查快照是否匹配：
    ```sh
    forge snapshot --check
    ```

### SEE ALSO

[forge](./forge.md), [forge test](./forge-test.md)
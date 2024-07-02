## forge config

### NAME

forge-config - 显示当前配置。

### SYNOPSIS

``forge config`` [*options*]

### DESCRIPTION

显示当前配置。

此命令可用于创建一个新的基本 `foundry.toml` 文件或查看当前设置的值，考虑环境变量和全局配置文件。

该命令支持 Forge 中几乎所有其他命令的标志，以允许覆盖显示的配置中的值。

### OPTIONS

#### 配置选项

`--basic`  
&nbsp;&nbsp;&nbsp;&nbsp;打印一个基本配置文件。

`--fix`  
&nbsp;&nbsp;&nbsp;&nbsp;尝试修复任何配置警告。

{{#include common-options.md}}

### EXAMPLES

1. 创建一个新的基本配置：
    ```sh
    forge config > foundry.toml
    ```

2. 在 `foundry.toml` 中启用 FFI：
    ```sh
    forge config --ffi > foundry.toml
    ```

### SEE ALSO

[forge](./forge.md)
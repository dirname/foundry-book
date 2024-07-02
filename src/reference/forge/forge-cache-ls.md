## `forge cache ls`

### NAME

forge-cache-ls - 显示从 `~/.foundry` 缓存的数据。

### SYNOPSIS

`forge cache ls` [*chains..*]

### DESCRIPTION

列出当前 `~/.foundry/cache` 文件夹中的内容。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 显示整个缓存（也是 ``forge cache ls`` 的别名）
    ```sh
    forge cache ls all
    ```

2. 显示特定链的缓存数据，通过名称
    ```sh
    forge cache ls rinkeby
    ```
   
3. 指定多个链
    ```sh
    forge cache ls rinkeby mainnet
    ```

### SEE ALSO
[forge](./forge.md), [forge cache](./forge-cache.md)
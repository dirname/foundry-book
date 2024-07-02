## forge update

### NAME

forge-update - 更新一个或多个依赖项。

### SYNOPSIS

``forge update`` [*options*] [*dep*]

### DESCRIPTION

更新一个或多个依赖项。

参数 *dep* 是要更新的依赖项的路径。
Forge 将更新到你在运行 [`forge install`](./forge-install.md) 时为该依赖项指定的 ref 上的最新版本。

如果没有提供参数，则更新所有依赖项。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 更新一个依赖项：
    ```sh
    forge update lib/solmate
    ```

2. 更新所有依赖项：
    ```sh
    forge update
    ```

### SEE ALSO

[forge](./forge.md), [forge install](./forge-install.md), [forge remove](./forge-remove.md)
## forge remove

### NAME

forge-remove - 移除一个或多个依赖项。

### SYNOPSIS

``forge remove`` [*options*] [*deps...*]

### DESCRIPTION

移除一个或多个依赖项。

依赖项可以是原始 URL（`https://foo.com/dep`）、GitHub 仓库的路径（`owner/repo`）或项目树中依赖项的路径。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 通过路径移除依赖项：
    ```sh
    forge remove lib/solmate
    ```

2. 通过 GitHub 仓库名称移除依赖项：
    ```sh
    forge remove dapphub/solmate
    ```

### SEE ALSO

[forge](./forge.md), [forge install](./forge-install.md), [forge update](./forge-update.md)
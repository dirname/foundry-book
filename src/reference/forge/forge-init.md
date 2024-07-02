## forge init

### NAME

forge-init - 创建一个新的 Forge 项目。

### SYNOPSIS

``forge init`` [*options*] [*root*]

### DESCRIPTION

在目录 *root* 中创建一个新的 Forge 项目（默认情况下为当前工作目录）。

默认模板创建以下项目布局：

```ignore
{{#include ../../output/hello_foundry/tree-with-files:output}}
```

然而，可以使用 `--template` 从另一个模板创建项目。

默认情况下，`forge init` 还会初始化一个新的 git 仓库，安装一些子模块并创建一个初始提交消息。

如果你不希望这种行为，可以传递 `--no-git`。

### OPTIONS

#### Init Options

`--force`  
&nbsp;&nbsp;&nbsp;&nbsp;即使指定的根目录不为空，也创建项目。

`-t` *template*  
`--template` *template*  
&nbsp;&nbsp;&nbsp;&nbsp;从哪个模板开始。

`--vscode`  
&nbsp;&nbsp;&nbsp;&nbsp;创建一个 `.vscode/settings.json` 文件，包含 Solidity 设置，并生成一个 `remappings.txt` 文件。

`--offline`  
&nbsp;&nbsp;&nbsp;&nbsp;不从网络安装依赖项。

#### VCS Options

`--no-commit`  
&nbsp;&nbsp;&nbsp;&nbsp;不创建初始提交。

`--no-git`  
&nbsp;&nbsp;&nbsp;&nbsp;不创建 git 仓库。

#### Display Options

`-q`  
`--quiet`  
&nbsp;&nbsp;&nbsp;&nbsp;不打印任何消息。

{{#include common-options.md}}

### EXAMPLES

1. 创建一个新的项目：
    ```sh
    forge init hello_foundry
    ```

2. 创建一个新的项目，但不创建 git 仓库：
    ```sh
    forge init --no-git hello_foundry
    ```

3. 强制在非空目录中创建一个新的项目：
    ```sh
    forge init --force 
    ```

### SEE ALSO

[forge](./forge.md)
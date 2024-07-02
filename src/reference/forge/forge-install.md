## forge install

### NAME

forge-install - 安装一个或多个依赖项。

### SYNOPSIS

``forge install`` [*options*] [*deps...*]

### DESCRIPTION

安装一个或多个依赖项。

依赖项作为 git 子模块安装。如果你不希望这种行为，请传递 `--no-git`。

如果没有提供参数，则安装现有的依赖项。

依赖项可以是原始 URL（`https://foo.com/dep`）、SSH URL（`git@github.com:owner/repo`）或 GitHub 仓库的路径（`owner/repo`）。此外，可以在依赖路径中添加 ref 以安装特定版本的依赖项。

ref 可以是：

- 分支：`owner/repo@master`
- 标签：`owner/repo@v1.2.3`
- 提交：`owner/repo@8e8128`

ref 默认为 `master`。

你还可以选择依赖项所在的文件夹名称。默认情况下，文件夹名称是仓库的名称。如果你想更改文件夹名称，请在依赖项前加上 `<folder>=`。

### OPTIONS

#### 项目选项

`--root` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录，或当前工作目录。

#### VCS 选项

`--no-commit`  
&nbsp;&nbsp;&nbsp;&nbsp;不要创建提交。

`--no-git`  
&nbsp;&nbsp;&nbsp;&nbsp;安装时不将依赖项添加为子模块。

#### 显示选项

`-q`  
`--quiet`  
&nbsp;&nbsp;&nbsp;&nbsp;不打印任何消息。

{{#include common-options.md}}

### EXAMPLES

1. 安装一个依赖项：
    ```sh
    forge install transmissions11/solmate
    ```

2. 安装特定版本的依赖项：
    ```sh
    forge install transmissions11/solmate@v7
    ```

3. 安装多个依赖项：
    ```sh
    forge install transmissions11/solmate@v7 OpenZeppelin/openzeppelin-contracts
    ```

4. 安装依赖项但不创建子模块：
    ```sh
    forge install --no-git transmissions11/solmate
    ```

5. 在特定文件夹中安装依赖项：
    ```sh
    forge install soulmate=transmissions11/solmate
    ```

### SEE ALSO

[forge](./forge.md), [forge update](./forge-update.md), [forge remove](./forge-remove.md)
## cast completions

### NAME

cast-completions - 生成 shell 自动补全脚本

### SYNOPSIS

``cast completions`` *shell*

### DESCRIPTION

为指定的 shell 生成自动补全脚本。

支持的 shell 有：

- bash
- elvish
- fish
- powershell
- zsh

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 为 zsh 生成 shell 自动补全脚本：
    ```sh
    cast completions zsh > $HOME/.oh-my-zsh/completions/_cast
    ```

### SEE ALSO

[cast](./cast.md)
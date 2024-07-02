## forge bind

### NAME

forge-bind - 生成智能合约的 Rust 绑定。

### SYNOPSIS

``forge bind`` [*options*]

### DESCRIPTION

使用 [ethers-rs](https://github.com/gakonst/ethers-rs) 生成智能合约的 Rust 绑定。

绑定是从项目的构件生成的，默认路径是 `./out/`。如果你想为不同目录中的构件生成绑定，可以传递 `--bindings-path <PATH>`。

有三种输出选项：

- 在 crate 中生成绑定（默认）
- 通过传递 `--module` 在模块中生成绑定
- 通过传递 `--single-file` 在单个文件中生成绑定

默认情况下，命令会检查现有绑定是否正确并相应退出。你可以通过传递 `--overwrite` 覆盖现有绑定。

### OPTIONS

#### Project Options

`-b` *path*  
`--bindings-path` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录或当前工作目录。

`--crate-name` *name*  
&nbsp;&nbsp;&nbsp;&nbsp;要生成的 Rust crate 的名称，如果你正在生成一个 crate（默认）。  
&nbsp;&nbsp;&nbsp;&nbsp;这应该是一个有效的 crates.io crate 名称。

&nbsp;&nbsp;&nbsp;&nbsp;默认值：foundry-contracts

`--crate-version` *semver*  
&nbsp;&nbsp;&nbsp;&nbsp;要生成的 Rust crate 的版本，如果你正在生成一个 crate（默认）。  
&nbsp;&nbsp;&nbsp;&nbsp;这应该是一个标准的 semver 版本字符串。

&nbsp;&nbsp;&nbsp;&nbsp;默认值：0.0.1

`--module`  
&nbsp;&nbsp;&nbsp;&nbsp;将绑定生成为模块而不是 crate。

`--single-file`  
&nbsp;&nbsp;&nbsp;&nbsp;将绑定生成为单个文件。

`--overwrite`  
&nbsp;&nbsp;&nbsp;&nbsp;覆盖现有的生成绑定。默认情况下，命令会检查绑定是否正确，然后退出。  
&nbsp;&nbsp;&nbsp;&nbsp;如果传递了 `--overwrite`，则会删除并覆盖绑定。

`--root` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录或当前工作目录。

`--skip-cargo-toml`  
&nbsp;&nbsp;&nbsp;&nbsp;跳过 Cargo.toml 一致性检查。  
&nbsp;&nbsp;&nbsp;&nbsp;这允许你在不放弃一致性检查的情况下管理 [ethers](https://github.com/gakonst/ethers-rs) 版本。  
&nbsp;&nbsp;&nbsp;&nbsp;例如，如果你使用 ethers 的额外功能，如 `ws`、`ipc` 或 `rustls`，并且出现 `ethers-providers` 版本不匹配的情况。

`--skip-build`  
&nbsp;&nbsp;&nbsp;&nbsp;在生成绑定之前跳过运行 forge build。  
&nbsp;&nbsp;&nbsp;&nbsp;这允许你跳过默认的 `forge build` 步骤，而是使用已经存在的构件生成绑定。

`--select-all`  
&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，所有以 `Test` 或 `Script` 结尾的合约都会被排除。这将显式地为所有合约生成绑定。与 `--select` 和 `--skip` 冲突。

`--select` *regex+*  
&nbsp;&nbsp;&nbsp;&nbsp;仅为名称匹配指定过滤器的合约生成绑定。与 `--skip` 冲突。

`--skip` *regex+*  
&nbsp;&nbsp;&nbsp;&nbsp;仅为名称不匹配指定过滤器的合约生成绑定。与 `--select` 冲突。

{{#include common-options.md}}

{{#include core-build-options.md}}

### SEE ALSO

[forge](./forge.md)
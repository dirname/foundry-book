## 安装

如果在安装过程中遇到任何问题，请查看[常见问题解答](../faq.md)。

### 预编译二进制文件

预编译二进制文件可以从[GitHub发布页面](https://github.com/foundry-rs/foundry/releases)获取。
这些文件最好通过使用[Foundryup](#using-foundryup)进行管理。

### 使用 Foundryup

Foundryup 是 Foundry 工具链安装程序。你可以在[这里](https://github.com/foundry-rs/foundry/blob/master/foundryup/README.md)了解更多信息。

打开终端并运行以下命令：

```sh
curl -L https://foundry.paradigm.xyz | bash
```

这将安装 Foundryup，然后按照屏幕上的指示操作，
这将使 `foundryup` 命令在你的 CLI 中可用。

运行 `foundryup` 本身将安装最新的（ nightly ）[预编译二进制文件](#precompiled-binaries)：`forge`、`cast`、`anvil` 和 `chisel`。
查看 `foundryup --help` 了解更多选项，例如从特定版本或提交安装。

> ℹ️ **注意**
>
> 如果你使用的是 Windows，你需要安装并使用 [Git BASH](https://gitforwindows.org/) 或 [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)，
> 作为你的终端，因为 Foundryup 目前不支持 Powershell 或 Cmd。

### 从源代码构建

#### 先决条件

你需要 [Rust](https://rust-lang.org) 编译器和 Cargo，即 Rust 包管理器。
安装两者的最简单方法是使用 [`rustup.rs`](https://rustup.rs/)。

Foundry 通常只支持在最新的稳定 Rust 版本上构建。
如果你有旧版本的 Rust，可以使用 `rustup` 进行更新：

```sh
rustup update stable
```

在 Windows 上，你还需要安装最新版本的 [Visual Studio](https://visualstudio.microsoft.com/downloads/)，
并选择“使用 C++ 的桌面开发”工作负载选项。

#### 构建

你可以使用不同的 [Foundryup](#using-foundryup) 标志：

```sh
foundryup --branch master
foundryup --path path/to/foundry
```

或者，通过使用单个 Cargo 命令：

```sh
cargo install --git https://github.com/foundry-rs/foundry --profile release --locked forge cast chisel anvil
```

或者，通过手动从 [Foundry 仓库](https://github.com/foundry-rs/foundry) 的本地副本构建：

```sh
# 克隆仓库
git clone https://github.com/foundry-rs/foundry.git
cd foundry
# 安装 Forge
cargo install --path ./crates/forge --profile release --force --locked
# 安装 Cast
cargo install --path ./crates/cast --profile release --force --locked
# 安装 Anvil
cargo install --path ./crates/anvil --profile release --force --locked
# 安装 Chisel
cargo install --path ./crates/chisel --profile release --force --locked
```

### 在 Github Action 中安装

请参阅 [foundry-rs/foundry-toolchain](https://github.com/foundry-rs/foundry-toolchain) GitHub Action。

### 使用 Docker 运行 Foundry

Foundry 也可以完全在 Docker 容器中使用。如果你没有安装 Docker，可以直接从 [Docker 的网站](https://docs.docker.com/get-docker/) 安装。

安装完成后，你可以通过运行以下命令下载最新版本：

```sh
docker pull ghcr.io/foundry-rs/foundry:latest
```

也可以在本地构建 Docker 镜像。从 Foundry 仓库中运行：

```sh
docker build -t foundry .
```

有关使用此镜像的示例和指南，请参阅 [Docker 教程部分](../tutorials/foundry-docker)。

> ℹ️ **注意**
>
> 一些机器（包括带有 M1 芯片的机器）可能无法在本地构建 Docker 镜像。这是一个已知问题。
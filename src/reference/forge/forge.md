## forge

### NAME

forge - 构建、测试、模糊测试、调试和部署 Solidity 合约。

### SYNOPSIS

`forge` [*options*] *command* [*args*]  
`forge` [*options*] `--version`  
`forge` [*options*] `--help`

### DESCRIPTION

这个程序是一组工具，用于构建、测试、模糊测试、调试和部署 Solidity 智能合约。

### COMMANDS

#### 通用命令

[forge help](./forge-help.md)  
&nbsp;&nbsp;&nbsp;&nbsp;显示关于 Forge 的帮助信息。

[forge completions](./forge-completions.md)  
&nbsp;&nbsp;&nbsp;&nbsp;生成 Forge 的 shell 自动补全。

#### 项目命令

[forge init](./forge-init.md)  
&nbsp;&nbsp;&nbsp;&nbsp;创建一个新的 Forge 项目。

[forge clone](./forge-clone.md)  
&nbsp;&nbsp;&nbsp;&nbsp;将链上已验证的合约克隆为 Forge 项目。

[forge install](./forge-install.md)  
&nbsp;&nbsp;&nbsp;&nbsp;安装一个或多个依赖项。

[forge update](./forge-update.md)  
&nbsp;&nbsp;&nbsp;&nbsp;更新一个或多个依赖项。

[forge remove](./forge-remove.md)  
&nbsp;&nbsp;&nbsp;&nbsp;移除一个或多个依赖项。

[forge config](./forge-config.md)  
&nbsp;&nbsp;&nbsp;&nbsp;显示当前配置。

[forge remappings](./forge-remappings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;获取项目自动推断的重映射。

[forge tree](./forge-tree.md)  
&nbsp;&nbsp;&nbsp;&nbsp;显示项目依赖图的树形可视化。

[forge geiger](./forge-geiger.md)  
&nbsp;&nbsp;&nbsp;&nbsp;检测 Foundry 项目及其依赖项中不安全作弊代码的使用情况。

#### 构建命令

[forge build](./forge-build.md)  
&nbsp;&nbsp;&nbsp;&nbsp;构建项目的智能合约。

[forge clean](./forge-clean.md)  
&nbsp;&nbsp;&nbsp;&nbsp;移除构建产物和缓存目录。

[forge inspect](./forge-inspect.md)  
&nbsp;&nbsp;&nbsp;&nbsp;获取智能合约的专用信息。

#### 测试命令

[forge test](./forge-test.md)  
&nbsp;&nbsp;&nbsp;&nbsp;运行项目的测试。

[forge snapshot](./forge-snapshot.md)  
&nbsp;&nbsp;&nbsp;&nbsp;创建每个测试的 gas 使用快照。

[forge coverage](./forge-coverage.md)  
&nbsp;&nbsp;&nbsp;&nbsp;生成覆盖率报告。

#### 部署命令

[forge create](./forge-create.md)  
&nbsp;&nbsp;&nbsp;&nbsp;部署智能合约。

[forge verify-contract](./forge-verify-contract.md)  
&nbsp;&nbsp;&nbsp;&nbsp;在 Etherscan 上验证智能合约。

[forge verify-check](./forge-verify-check.md)  
&nbsp;&nbsp;&nbsp;&nbsp;检查 Etherscan 上的验证状态。

[forge flatten](./forge-flatten.md)  
&nbsp;&nbsp;&nbsp;&nbsp;将源文件及其所有导入展平为一个文件。

#### 实用命令

[forge debug](./forge-debug.md)  
&nbsp;&nbsp;&nbsp;&nbsp;将单个智能合约作为脚本进行调试。

[forge bind](./forge-bind.md)  
&nbsp;&nbsp;&nbsp;&nbsp;为智能合约生成 Rust 绑定。

[forge cache](./forge-cache.md)  
&nbsp;&nbsp;&nbsp;&nbsp;管理 Foundry 缓存。

[forge cache clean](./forge-cache-clean.md)  
&nbsp;&nbsp;&nbsp;&nbsp;清除 `~/.foundry` 中的缓存数据。

[forge cache ls](./forge-cache-ls.md)  
&nbsp;&nbsp;&nbsp;&nbsp;显示 `~/.foundry` 中的缓存数据。

[forge script](./forge-script.md)  
&nbsp;&nbsp;&nbsp;&nbsp;将智能合约作为脚本运行，构建可以发送到链上的交易。

[forge upload-selectors](./forge-upload-selectors.md)  
&nbsp;&nbsp;&nbsp;&nbsp;将给定合约的 ABI 上传到 https://sig.eth.samczsun.com 函数选择器数据库。

[forge doc](./forge-doc.md)  
&nbsp;&nbsp;&nbsp;&nbsp;为 Solidity 源文件生成文档。

### OPTIONS

#### 特殊选项

`-V`  
`--version`  
&nbsp;&nbsp;&nbsp;&nbsp;打印版本信息并退出。

{{#include common-options.md}}

### FILES

`~/.foundry/`  
&nbsp;&nbsp;&nbsp;&nbsp;Foundry 的默认“主”目录位置，存储各种文件。

`~/.foundry/bin/`  
&nbsp;&nbsp;&nbsp;&nbsp;使用 `foundryup` 安装的二进制文件将位于此处。

`~/.foundry/cache/`  
&nbsp;&nbsp;&nbsp;&nbsp;Forge 的缓存目录，存储缓存的区块数据等。

`~/.foundry/foundry.toml`  
&nbsp;&nbsp;&nbsp;&nbsp;全局 [Foundry 配置](../config/overview.md)。

`~/.svm`  
&nbsp;&nbsp;&nbsp;&nbsp;Forge 管理的 solc 二进制文件的位置。

### EXAMPLES

1. 创建一个新的 Forge 项目：
    ```sh
    forge init hello_foundry
    ```

2. 构建项目：
    ```sh
    forge build
    ```

3. 运行项目的测试：
    ```sh
    forge test
    ```

### BUGS

查看 <https://github.com/foundry-rs/foundry/issues> 获取问题。
## Soldeer 作为包管理器

如[这里](./dependencies)所述，Foundry 迄今为止一直使用 git 子模块来处理依赖关系。

随着项目变得越来越复杂，对原生包管理器的需求开始显现。

一个新的方法正在开发中，[soldeer.xyz](https://soldeer.xyz)，这是一个用 Rust 构建的 Solidity 原生依赖管理器，并且是开源的（查看仓库 [https://github.com/mario-eth/soldeer](https://github.com/mario-eth/soldeer)）。

### 添加依赖

#### 添加存储在中央仓库的依赖

要添加依赖，您可以访问 [soldeer.xyz](https://soldeer.xyz) 并搜索您想要添加的依赖（例如，openzeppelin 0.5.2）。

![image](https://i.postimg.cc/Hm6R8MTs/Unknown-413.png)

然后只需运行 forge 命令：
```bash
forge soldeer install @openzeppelin-contracts~5.0.2
```

这将从中央仓库下载依赖并安装到 `dependencies` 目录中。

Soldeer 可以管理两种类型的依赖配置：使用 `soldeer.toml` 或嵌入在 `foundry.toml` 中。为了与 Foundry 一起工作，您必须在 `foundry.toml` 中定义 `[dependencies]` 配置。这将告诉 `soldeer CLI` 在该处定义已安装的依赖。例如：

```toml
# 完整参考 https://github.com/foundry-rs/foundry/tree/master/crates/config

[profile.default]
auto_detect_solc = false 
bytecode_hash = "none" 
fuzz = { runs = 1_000 } 
libs = ["dependencies"] # <= 这很重要，需要添加
gas_reports = ["*"] 

[dependencies] # <= 依赖将添加到此配置下
"@openzeppelin-contracts" = { version = "5.0.2" }
"@uniswap-universal-router" = { version = "1.6.0" }
"@prb-math" = { version = "4.0.2" }
forge-std = { version = "1.8.1" }
```

#### 添加存储在特定链接的依赖

如果中央仓库没有某个依赖，您可以通过提供一个 zip 存档链接来安装它。

例如：
```bash
forge soldeer install @custom-dependency~1.0.0 https://my-website.com/custom-dependency-1-0-0.zip
```

上述命令将尝试从提供的链接下载依赖并将其安装为正常依赖。为此，您将在配置中看到一个额外的字段 `path`。

例如：
```toml
[dependencies]
"@custom-dependency" = { version = "1.0.0", path = "https://my-website.com/custom-dependency-1-0-0.zip" }
```

### 重映射依赖

依赖的重映射是自动执行的，Soldeer 会将依赖添加到 `remappings.txt` 中。

例如：
```bash
@openzeppelin-contracts-5.0.2=dependencies/@openzeppelin-contracts-5.0.2
@uniswap-universal-router-1.6.0=dependencies/@uniswap-universal-router-1.6.0
@prb-math-4.0.2=dependencies/@prb-math-4.0.2
@forge-std-1.8.1=dependencies/forge-std-1.8.1
```
这些重映射意味着：

- 要从 `forge-std` 导入，我们会写：`import "forge-std-1.8.1/Contract.sol";`
- 要从 `@openzeppelin-contracts` 导入，我们会写：`import "@openzeppelin-contracts-5.0.2/Contract.sol";`

### 更新依赖

因为 Soldeer 在配置文件（foundry 或 soldeer toml）中指定依赖，所以在团队中共享依赖配置变得更加容易。

例如，在 git 仓库中有一个 Foundry 配置文件，可以拉取仓库然后运行 `forge soldeer update`。此命令将自动安装在 `[dependencies]` 标签下指定的所有依赖。

```toml
# 完整参考 https://github.com/foundry-rs/foundry/tree/master/crates/config

[profile.default]
auto_detect_solc = false 
bytecode_hash = "none" 
fuzz = { runs = 1_000 } 
gas_reports = ["*"] # <= 这很重要，需要添加

[dependencies] # <= 依赖将添加到此配置下
"@openzeppelin-contracts" = { version = "5.0.2" }
"@uniswap-universal-router" = { version = "1.6.0" }
"@prb-math" = { version = "4.0.2" }
forge-std = { version = "1.8.1" }
```

### 移除依赖

要移除依赖，您必须手动从 `dependencies` 目录和 `[dependencies]` 标签中删除它。

### 将新版本推送到中央仓库

Soldeer 类似于 npmjs/crates.io，鼓励所有开发者将他们的项目发布到中央仓库。

要做到这一点，您必须前往 [soldeer.xyz](https://soldeer.xyz)，创建一个账户，验证它，然后

![image](https://i.postimg.cc/G3VDpN2S/s1.png) 

只需添加一个新项目

![image](https://i.postimg.cc/rsBRYd3L/s2.png) 

项目创建后，您可以进入项目源并：
- 创建一个 `.soldeerignore` 文件，该文件类似于 `.gitignore`，用于排除不需要的文件。
- 运行 `forge soldeer login` 登录到您的账户。
- 在您想要推送到中央仓库的目录中运行 `forge soldeer push my-project~1.0.0`。

如果您想推送特定目录而不是当前终端所在的目录，可以使用 `forge soldeer push my-project~1.0.0 /path/to/directory`。

> **警告** ⚠️
> - 一旦项目创建，无法删除。
> - 一旦版本推送，无法删除。
> - 您不能推送相同的版本两次。
> - 在终端中运行的项目名称命令必须与在 Soldeer 网站上创建的项目名称匹配。
> - 我们鼓励大家在导入依赖时使用版本固定，这将有助于通过确切知道正在使用的依赖版本来确保代码安全。此外，它将帮助安全研究人员进行工作。
例如，不要使用 `import '@openzeppelin-contracts/token/ERC20.sol'`，而应使用 `import '@openzeppelin-contracts-5.0.2/token/ERC20.sol'`。

- 如果中央仓库中没有某个包，您可以在 [Soldeer Repository](https://github.com/mario-eth/soldeer/issues) 中打开一个问题，团队将考虑添加它。
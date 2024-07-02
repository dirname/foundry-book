## 依赖

Forge 默认使用 [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) 管理依赖，这意味着它可以与任何包含智能合约的 GitHub 仓库一起工作。

### 添加依赖

要添加依赖，运行 [`forge install`](../reference/forge/forge-install.md)：

```sh
{{#include ../output/deps/forge-install:all}}
```

这将拉取 `solmate` 库，在 git 中暂存 `.gitmodules` 文件，并提交一条消息为 "Installed solmate" 的提交。

如果我们现在检查 `lib` 文件夹：

```sh
{{#include ../output/deps/tree:all}}
```

我们可以看到 Forge 已经安装了 `solmate`！

默认情况下，`forge install` 安装最新的 master 分支版本。如果你想安装特定的标签或提交，可以这样做：

```sh
$ forge install transmissions11/solmate@v7
```

### 重映射依赖

Forge 可以重映射依赖，使其更容易导入。Forge 会自动尝试为你推导一些重映射：

```sh
{{#include ../output/deps/forge-remappings:all}}
```

这些重映射意味着：

- 要从 `forge-std` 导入，我们会写：`import "forge-std/Contract.sol";`
- 要从 `ds-test` 导入，我们会写：`import "ds-test/Contract.sol";`
- 要从 `solmate` 导入，我们会写：`import "solmate/Contract.sol";`
- 要从 `weird-erc20` 导入，我们会写：`import "weird-erc20/Contract.sol";`

你可以通过在项目根目录下创建一个 `remappings.txt` 文件来自定义这些重映射。

让我们创建一个名为 `solmate-utils` 的重映射，指向 solmate 仓库中的 `utils` 文件夹！

```sh
@solmate-utils/=lib/solmate/src/utils/
```

你也可以在 `foundry.toml` 中设置重映射。

```toml
remappings = [
    "@solmate-utils/=lib/solmate/src/utils/",
]
```

现在我们可以像这样导入 solmate 仓库中 `src/utils` 中的任何合约：

```solidity
import "@solmate-utils/LibString.sol";
```

### 更新依赖

你可以使用 [`forge update <dep>`](../reference/forge/forge-update.md) 将特定依赖更新到你指定的版本的最新提交。例如，如果我们想从之前安装的 master 版本的 `solmate` 拉取最新提交，我们会运行：

```sh
$ forge update lib/solmate
```

或者，你可以通过只运行 `forge update` 来一次性更新所有依赖。

### 移除依赖

你可以使用 [`forge remove <deps>...`](../reference/forge/forge-remove.md) 移除依赖，其中 `<deps>` 是依赖的完整路径或仅名称。例如，要移除 `solmate`，这两个命令是等效的：

```ignore
$ forge remove solmate
# ... 等效于 ...
$ forge remove lib/solmate
```

### Hardhat 兼容性

Forge 还支持 Hardhat 风格的项目，其中依赖是 npm 包（存储在 `node_modules` 中），合约存储在 `contracts` 中，而不是 `src` 中。

要启用 Hardhat 兼容模式，请传递 `--hh` 标志。
## 集成到 VSCode

您可以通过安装 [VSCode Solidity 扩展](https://github.com/juanfranblanco/vscode-solidity) 来为 Visual Studio Code 获取 Solidity 支持。

为了让扩展与 Foundry 更好地配合，您可能需要调整一些设置。

### 1. 重映射

您可能希望将重映射放在 `remappings.txt` 中。

如果它们已经在 `foundry.toml` 中，请将它们复制过来并使用 `remappings.txt` 代替。如果您只是使用 Foundry 提供的自动生成的重映射，请运行 `forge remappings > remappings.txt`。

### 2. 依赖项

您可能需要将以下内容添加到您的 `.vscode/settings.json` 中，以便扩展能够找到您的依赖项：

```json
{
  "solidity.packageDefaultDependenciesContractsDirectory": "src",
  "solidity.packageDefaultDependenciesDirectory": "lib"
}
```

其中 `src` 是源代码目录，`lib` 是您的依赖项目录。

### 3. 格式化器

要启用 Foundry 内置的格式化器，以便在保存时自动格式化您的代码，您可以将以下设置添加到您的 `.vscode/settings.json` 中：

```json
{
  "editor.formatOnSave": true,
  "[solidity]": {
    "editor.defaultFormatter": "JuanBlanco.solidity" 
  },
  "solidity.formatter": "forge",
}
```

要配置格式化器设置，请参考 [格式化器](../reference/config/formatter.md) 参考。

### 4. Solc 版本

最后，建议指定一个 Solidity 编译器版本：

```json
"solidity.compileUsingRemoteVersion": "v0.8.17"
```

为了使 Foundry 与所选版本保持一致，请将以下内容添加到您的 `foundry.toml` 中的 `default` 配置文件中。

```toml
solc = "0.8.17"
```
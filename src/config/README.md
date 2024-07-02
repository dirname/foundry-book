## 使用 `foundry.toml` 进行配置

Forge 可以使用一个名为 `foundry.toml` 的配置文件进行配置，该文件放置在项目的根目录中。

配置可以通过配置文件进行命名空间划分。默认的配置文件名为 `default`，所有其他配置文件都继承自该配置文件。您可以自由地自定义 `default` 配置文件，并根据需要添加任意数量的新配置文件。

此外，您可以在主目录中创建一个全局的 `foundry.toml` 文件。

让我们来看一个包含两个配置文件的配置文件：默认配置文件，始终启用优化器，以及一个 CI 配置文件，始终显示跟踪信息：

```toml
[profile.default]
optimizer = true
optimizer_runs = 20_000

[profile.ci]
verbosity = 4
```

运行 `forge` 时，可以使用 `FOUNDRY_PROFILE` 环境变量指定要使用的配置文件。

### 独立部分

除了配置文件部分外，配置文件还可以包含独立部分（如 `[fmt]`、`[fuzz]`、`[invariant]` 等）。默认情况下，每个独立部分都属于 `default` 配置文件。
即 `[fmt]` 等同于 `[profile.default.fmt]`。

要为 `default` 以外的其他配置文件配置独立部分，请使用语法 `[profile.<profile name>.<standalone>]`。
即 `[profile.ci.fuzz]`。

<br>

> 📚 **参考**
> 
> 请参阅 [`foundry.toml` 参考](../reference/config/) 以获取完整的配置概览。
## 配置概述

Foundry 的配置系统允许您配置其工具。

### 配置文件

配置可以任意命名空间到配置文件中。默认配置文件名为 `default`，所有其他配置文件都继承自此配置文件的值。配置文件在 `profile` 映射中定义。

要添加一个名为 `local` 的配置文件，您可以添加：

```toml
[profile.local]
```

您可以通过设置 `FOUNDRY_PROFILE` 环境变量来选择要使用的配置文件。

### 全局配置

您可以在 `~/.foundry` 文件夹中创建一个 `foundry.toml` 文件来全局配置 Foundry。

### 环境变量

配置可以通过带有 `FOUNDRY_` 和 `DAPP_` 前缀的环境变量进行覆盖。

例外情况是：

- `FOUNDRY_FFI`, `DAPP_FFI`, `DAPP_TEST_FFI`
- `FOUNDRY_PROFILE`
- `FOUNDRY_REMAPPINGS`, `DAPP_REMAPPINGS`
- `FOUNDRY_LIBRARIES`, `DAPP_LIBRARIES`
- `FOUNDRY_FS_PERMISSIONS`, `DAPP_FS_PERMISSIONS`, `DAPP_TEST_FS_PERMISSIONS`
- `DAPP_TEST_CACHE`
- `DAPP_TEST_FUZZ_RUNS`
- `DAPP_TEST_FUZZ_DEPTH`

### 配置格式

配置文件使用 [TOML 格式](https://toml.io) 编写，在各个部分中包含简单的键值对。

本页详细描述了每个配置键。要查看默认值，请参阅本文档中的特定键，或查看 [默认配置](/static/config.default.toml)。

### 配置键

本节记录所有配置键。所有配置键必须位于一个配置文件下，例如 `default`。
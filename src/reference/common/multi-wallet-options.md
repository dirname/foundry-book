```markdown
# 多钱包选项

Foundry 支持多种钱包选项，包括本地密钥库、硬件钱包和远程签名者。

## 本地密钥库

Foundry 支持使用本地密钥库文件进行签名。你可以通过以下方式配置密钥库文件：

```toml
[profile.default.wallet]
keystore = "path/to/keystore"
password = "path/to/password"
```

## 硬件钱包

Foundry 支持使用硬件钱包进行签名。你可以通过以下方式配置硬件钱包：

```toml
[profile.default.wallet]
hardware = "ledger"
```

## 远程签名者

Foundry 支持使用远程签名者进行签名。你可以通过以下方式配置远程签名者：

```toml
[profile.default.wallet]
remote = "http://localhost:8545"
```
```
```markdown
# 钱包选项

Foundry 支持多种钱包选项，包括原始私钥、密钥库文件、硬件钱包和远程签名者。

## 原始私钥

你可以使用 `PRIVATE_KEY` 环境变量来指定一个原始私钥。

```sh
export PRIVATE_KEY=<your_private_key>
```

你也可以在 `foundry.toml` 文件中配置它：

```toml
[profile.default]
private_key = "<your_private_key>"
```

## 密钥库文件

Foundry 支持使用密钥库文件进行签名。你可以通过 `ETH_KEYSTORE` 环境变量指定密钥库文件的路径，并通过 `ETH_PASSWORD` 环境变量提供密码。

```sh
export ETH_KEYSTORE=<path_to_keystore>
export ETH_PASSWORD=<your_keystore_password>
```

你也可以在 `foundry.toml` 文件中配置它：

```toml
[profile.default]
keystore = "<path_to_keystore>"
password = "<your_keystore_password>"
```

## 硬件钱包

Foundry 支持使用硬件钱包进行签名。你可以通过 `ETH_HW_WALLET` 环境变量指定硬件钱包的类型。

```sh
export ETH_HW_WALLET=<hardware_wallet_type>
```

你也可以在 `foundry.toml` 文件中配置它：

```toml
[profile.default]
hardware_wallet = "<hardware_wallet_type>"
```

## 远程签名者

Foundry 支持使用远程签名者进行签名。你可以通过 `ETH_RPC_URL` 环境变量指定远程签名者的 RPC URL。

```sh
export ETH_RPC_URL=<remote_signer_rpc_url>
```

你也可以在 `foundry.toml` 文件中配置它：

```toml
[profile.default]
rpc_url = "<remote_signer_rpc_url>"
```
```
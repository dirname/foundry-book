```markdown
# cast wallet decrypt-keystore

解密 keystore 文件以获取私钥

```bash
$ cast wallet decrypt-keystore --help
Usage: cast wallet decrypt-keystore [OPTIONS] <ACCOUNT_NAME>

Arguments:
  <ACCOUNT_NAME>  在 keystore 中的账户名称

Options:
  -k, --keystore-dir <KEYSTORE_DIR>  如果未提供，keystore 将尝试在默认的 keystores 目录（~/.foundry/keystores）中定位
      --unsafe-password <PASSWORD>   明文密码用于 JSON keystore。这是不安全的，我们建议使用默认的隐藏密码提示 [env: CAST_UNSAFE_PASSWORD=]
  -h, --help                         打印帮助信息
```
```
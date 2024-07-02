```json
{
  "id": "cast-wallet-new",
  "title": "cast wallet new",
  "description": "创建一个新的随机密钥对"
}
```

```bash
$ cast wallet new --help
用法: cast wallet new [选项] [路径]

参数:
  [路径]
          如果提供，则密钥对将写入加密的 JSON 密钥库

选项:
  -p, --password
          触发 JSON 密钥库的隐藏密码提示。
          
          已弃用: 现在默认提示隐藏密码。

      --unsafe-password <PASSWORD>
          JSON 密钥库的明文密码。
          
          这是不安全的用法，我们建议使用 --password。
          
          [env: CAST_PASSWORD=]

  -n, --number <NUMBER>
          要生成的钱包数量
          
          [默认值: 1]

  -j, --json
          以 JSON 格式输出生成的钱包

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```json
{
  "id": "cast-wallet-vanity",
  "title": "生成一个自定义地址",
  "content": "```bash\n$ cast wallet vanity --help\nUsage: cast wallet vanity [OPTIONS]\n\nOptions:\n      --starts-with <HEX>\n          前缀正则表达式模式或十六进制字符串\n\n      --ends-with <HEX>\n          后缀正则表达式模式或十六进制字符串\n\n      --nonce <NONCE>\n          使用指定的nonce生成由生成的密钥对创建的自定义合约地址\n\n      --save-path <PATH>\n          保存生成的自定义合约地址的路径。\n          \n          如果提供，生成的自定义地址将附加到指定文件中的JSON数组。\n\n  -h, --help\n          打印帮助信息（使用'-h'查看摘要）\n```"
}
```
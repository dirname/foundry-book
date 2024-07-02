```markdown
# cast call --create

忽略地址字段并模拟创建合约

```bash
$ cast call --create --help
Usage: cast call --create [OPTIONS] <CODE> [SIG] [ARGS]...

Arguments:
  <CODE>
          合约的字节码

  [SIG]
          构造函数的签名

  [ARGS]...
          构造函数的参数

Options:
      --value <VALUE>
          交易中发送的以太币数量。
          
          可以以wei为单位指定，或以带单位类型的字符串指定。
          
          示例：1ether, 10gwei, 0.01ether

  -h, --help
          打印帮助信息（使用'-h'查看摘要）
```
```
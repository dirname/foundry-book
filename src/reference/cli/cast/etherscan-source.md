```markdown
# cast etherscan-source

从Etherscan获取合约的源代码

```bash
$ cast etherscan-source --help
Usage: cast etherscan-source [OPTIONS] <ADDRESS>

Arguments:
  <ADDRESS>  合约的地址

Options:
  -f, --flatten                  是否展平源代码
  -d <DIRECTORY>                 输出目录/文件以展开源代码树
  -e, --etherscan-api-key <KEY>  Etherscan（或等效）API密钥 [env: ETHERSCAN_API_KEY=]
  -c, --chain <CHAIN>            链名称或EIP-155链ID [env: CHAIN=]
  -h, --help                     打印帮助信息
```
```
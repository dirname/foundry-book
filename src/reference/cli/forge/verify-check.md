```markdown
# forge verify-check

检查在Etherscan上的验证状态

```bash
$ forge verify-check --help
Usage: forge verify-check [OPTIONS] <ID>

Arguments:
  <ID>
          验证ID。
          
          对于Etherscan - 提交GUID。
          
          对于Sourcify - 合约地址。

Options:
      --retries <RETRIES>
          重试验证的尝试次数
          
          [default: 5]

      --delay <DELAY>
          在验证尝试之间应用的可选延迟，以秒为单位
          
          [default: 5]

  -e, --etherscan-api-key <KEY>
          Etherscan（或等效）API密钥
          
          [env: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或EIP-155链ID
          
          [env: CHAIN=]

  -h, --help
          打印帮助信息（使用'-h'查看摘要）

Verifier options:
      --verifier <VERIFIER>
          使用的合约验证提供商
          
          [default: etherscan]
          [possible values: etherscan, sourcify, blockscout, oklink]

      --verifier-url <VERIFIER_URL>
          如果使用自定义提供商，则为验证URL
          
          [env: VERIFIER_URL=]
```
```
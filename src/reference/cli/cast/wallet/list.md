```markdown
# cast wallet list

列出密钥库默认目录中的所有账户

```bash
$ cast wallet list --help
Usage: cast wallet list [OPTIONS]

Options:
      --dir [<DIR>]                列出密钥库目录中的所有账户。如果没有提供路径，则使用默认密钥库目录
  -l, --ledger                      列出来自 Ledger 硬件钱包的账户
  -t, --trezor                      列出来自 Trezor 硬件钱包的账户
      --aws                         列出来自 AWS KMS 的账户
      --all                         列出所有已配置的账户
  -m, --max-senders <MAX_SENDERS>   从硬件钱包中显示的最大地址数量 [默认: 3]
  -h, --help                        打印帮助信息
```
```
```markdown
# cast wallet sign

签名消息或类型化数据

```bash
$ cast wallet sign --help
Usage: cast wallet sign [OPTIONS] <MESSAGE>

Arguments:
  <MESSAGE>
          要签名的消息、类型化数据或哈希。
          
          以0x开头的消息预期为十六进制编码，会在签名前进行解码。
          
          消息会在签名前加上以太坊签名消息头并进行哈希处理，除非提供了 `--no-hash`。
          
          类型化数据可以作为JSON字符串或文件名提供。使用 `--data` 标志表示消息是类型化数据的字符串。使用 `--data --from-file` 表示消息是包含类型化数据的文件名。数据将按照EIP712规范组合并进行哈希处理后再签名。数据应格式化为JSON。

Options:
      --data
          将消息视为JSON类型化数据

      --from-file
          将消息视为包含JSON类型化数据的文件。需要 `--data`

      --no-hash
          将消息视为原始的32字节哈希并直接签名，不再进行哈希处理

  -h, --help
          打印帮助信息（使用 `-h` 查看摘要）

Wallet options - raw:
  -f, --from <ADDRESS>
          发送者账户
          
          [env: ETH_FROM=]

  -i, --interactive
          打开一个交互式提示以输入您的私钥

      --private-key <RAW_PRIVATE_KEY>
          使用提供的私钥

      --mnemonic <MNEMONIC>
          使用指定路径的助记词或助记词文件

      --mnemonic-passphrase <PASSPHRASE>
          使用BIP39助记词的密码

      --mnemonic-derivation-path <PATH>
          钱包派生路径。
          
          适用于 --mnemonic-path 和硬件钱包。

      --mnemonic-index <INDEX>
          使用给定助记词索引的私钥。
          
          与 --mnemonic-path 一起使用。
          
          [default: 0]

Wallet options - keystore:
      --keystore <PATH>
          使用给定文件夹或文件中的keystore
          
          [env: ETH_KEYSTORE=]

      --account <ACCOUNT_NAME>
          通过文件名从默认keystore文件夹（~/.foundry/keystores）中使用keystore
          
          [env: ETH_KEYSTORE_ACCOUNT=]

      --password <PASSWORD>
          keystore密码。
          
          与 --keystore 一起使用。

      --password-file <PASSWORD_FILE>
          keystore密码文件路径。
          
          与 --keystore 一起使用。
          
          [env: ETH_PASSWORD=]

Wallet options - hardware wallet:
  -l, --ledger
          使用Ledger硬件钱包

  -t, --trezor
          使用Trezor硬件钱包

Wallet options - remote:
      --aws
          使用AWS Key Management Service
```
```
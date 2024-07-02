```markdown
# cast wallet private-key

从助记词派生私钥

```bash
$ cast wallet private-key --help
Usage: cast wallet private-key [OPTIONS] [MNEMONIC] [MNEMONIC_INDEX_OR_DERIVATION_PATH]

Arguments:
  [MNEMONIC]
          如果提供，私钥将从指定的助记词短语派生

  [MNEMONIC_INDEX_OR_DERIVATION_PATH]
          如果提供，私钥将使用指定的助记词索引（如果是整数）或派生路径派生

Options:
  -v, --verbose
          详细模式，打印地址和私钥

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Wallet options - raw:
  -f, --from <ADDRESS>
          发送者账户
          
          [env: ETH_FROM=]

  -i, --interactive
          打开一个交互式提示以输入您的私钥

      --private-key <RAW_PRIVATE_KEY>
          使用提供的私钥

      --mnemonic <MNEMONIC>
          使用指定路径的助记词短语或助记词文件

      --mnemonic-passphrase <PASSPHRASE>
          使用 BIP39 助记词密码

      --mnemonic-derivation-path <PATH>
          钱包派生路径。
          
          适用于 --mnemonic-path 和硬件钱包。

      --mnemonic-index <INDEX>
          使用给定助记词索引的私钥。
          
          与 --mnemonic-path 一起使用。
          
          [default: 0]

Wallet options - keystore:
      --keystore <PATH>
          使用指定文件夹或文件中的 keystore
          
          [env: ETH_KEYSTORE=]

      --account <ACCOUNT_NAME>
          通过其文件名从默认 keystores 文件夹（~/.foundry/keystores）中使用 keystore
          
          [env: ETH_KEYSTORE_ACCOUNT=]

      --password <PASSWORD>
          keystore 密码。
          
          与 --keystore 一起使用。

      --password-file <PASSWORD_FILE>
          keystore 密码文件路径。
          
          与 --keystore 一起使用。
          
          [env: ETH_PASSWORD=]

Wallet options - hardware wallet:
  -l, --ledger
          使用 Ledger 硬件钱包

  -t, --trezor
          使用 Trezor 硬件钱包

Wallet options - remote:
      --aws
          使用 AWS Key Management Service
```
```
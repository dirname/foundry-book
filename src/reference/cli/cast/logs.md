```markdown
# 查询日志

通过签名或主题获取日志

```bash
$ cast logs --help
Usage: cast logs [OPTIONS] [SIG_OR_TOPIC] [TOPICS_OR_ARGS]...

Arguments:
  [SIG_OR_TOPIC]
          要过滤日志的事件签名，将转换为第一个主题，或是一个要过滤的主题

  [TOPICS_OR_ARGS]...
          如果与签名一起使用，则是要过滤的事件的索引字段。否则，是过滤器的剩余主题

Options:
      --from-block <FROM_BLOCK>
          开始查询的区块高度。
          
          也可以是 earliest, finalized, safe, latest, 或 pending 这些标签。

      --to-block <TO_BLOCK>
          停止查询的区块高度。
          
          也可以是 earliest, finalized, safe, latest, 或 pending 这些标签。

      --address <ADDRESS>
          要过滤的合约地址

      --subscribe
          如果 RPC 类型和端点支持 `eth_subscribe`，则流式传输日志而不是打印并退出。将继续直到中断或达到 TO_BLOCK

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Display options:
  -j, --json
          以 JSON 格式打印日志

Ethereum options:
  -r, --rpc-url <URL>
          RPC 端点
          
          [env: ETH_RPC_URL=]

      --flashbots
          使用 Flashbots RPC URL 并启用快速模式（<https://rpc.flashbots.net/fast>）。
          
          这将私密地与所有注册的构建者共享交易。
          
          参见：<https://docs.flashbots.net/flashbots-protect/quick-start#faster-transactions>

      --jwt-secret <JWT_SECRET>
          RPC 端点的 JWT 密钥。
          
          JWT 密钥将用于创建 RPC 的 JWT。例如，以下可以用于模拟 CL `engine_forkchoiceUpdated` 调用：
          
          cast rpc --jwt-secret <JWT_SECRET> engine_forkchoiceUpdatedV2
          '["0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc"]'
          
          [env: ETH_RPC_JWT_SECRET=]

  -e, --etherscan-api-key <KEY>
          Etherscan（或等效）API 密钥
          
          [env: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [env: CHAIN=]

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
          使用 BIP39 助记词的密码

      --mnemonic-derivation-path <PATH>
          钱包派生路径。
          
          适用于 --mnemonic-path 和硬件钱包。

      --mnemonic-index <INDEX>
          使用给定助记词索引的私钥。
          
          与 --mnemonic-path 一起使用。
          
          [default: 0]

Wallet options - keystore:
      --keystore <PATH>
          使用给定文件夹或文件中的 keystore
          
          [env: ETH_KEYSTORE=]

      --account <ACCOUNT_NAME>
          通过文件名从默认 keystores 文件夹（~/.foundry/keystores）中使用 keystore
          
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
          使用 AWS 密钥管理服务
```
```
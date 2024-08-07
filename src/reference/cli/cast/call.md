```markdown
# cast call

在不发布交易的情况下对账户执行调用

```bash
$ cast call --help
Usage: cast call [OPTIONS] [TO] [SIG] [ARGS]... [COMMAND]

Commands:
  --create  忽略地址字段并模拟创建合约
  help      打印此消息或给定子命令的帮助信息

Arguments:
  [TO]
          交易的接收方

  [SIG]
          要调用的函数的签名

  [ARGS]...
          要调用的函数的参数

Options:
      --data <DATA>
          交易的数据

      --trace
          分叉远程RPC，在本地执行交易并打印追踪信息

      --debug
          打开交互式调试器。只能与 `--trace` 一起使用

      --labels <LABELS>
          应用于追踪的标签；格式：`address:label`。只能与 `--trace` 一起使用

      --evm-version <EVM_VERSION>
          使用的EVM版本。只能与 `--trace` 一起使用

  -b, --block <BLOCK>
          查询的区块高度。
          
          也可以是标签 earliest, finalized, safe, latest, 或 pending。

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Transaction options:
      --gas-limit <GAS_LIMIT>
          交易的 gas 限制
          
          [env: ETH_GAS_LIMIT=]

      --gas-price <PRICE>
          传统交易的 gas 价格，或 EIP1559 交易的每 gas 最大费用
          
          [env: ETH_GAS_PRICE=]

      --priority-gas-price <PRICE>
          EIP1559 交易的每 gas 最大优先费用
          
          [env: ETH_PRIORITY_GAS_PRICE=]

      --value <VALUE>
          交易中发送的以太币，可以是以 wei 为单位指定，或以带单位的字符串指定。
          
          示例：1ether, 10gwei, 0.01ether

      --nonce <NONCE>
          交易的 nonce

      --legacy
          发送传统交易而不是 EIP1559 交易。
          
          对于没有 EIP1559 的常见网络会自动启用。

      --blob
          发送 EIP-4844 blob 交易

      --blob-gas-price <BLOB_PRICE>
          EIP-4844 blob 交易的 gas 价格
          
          [env: ETH_BLOB_GAS_PRICE=]

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
          发送方账户
          
          [env: ETH_FROM=]

  -i, --interactive
          打开交互式提示以输入您的私钥

      --private-key <RAW_PRIVATE_KEY>
          使用提供的私钥

      --mnemonic <MNEMONIC>
          使用指定路径的助记词文件的助记词短语

      --mnemonic-passphrase <PASSPHRASE>
          使用助记词的 BIP39 密码短语

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
          使用默认 keystores 文件夹（~/.foundry/keystores）中按文件名指定的 keystore
          
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
# cast compute-address

计算给定nonce和部署者地址的合约地址

```bash
$ cast compute-address --help
Usage: cast compute-address [OPTIONS] [ADDRESS]

Arguments:
  [ADDRESS]
          部署者地址

Options:
      --nonce <NONCE>
          部署者地址的nonce

  -r, --rpc-url <URL>
          RPC 端点
          
          [env: ETH_RPC_URL=]

      --flashbots
          使用Flashbots RPC URL并启用快速模式（<https://rpc.flashbots.net/fast>）。
          
          这将私密地与所有注册的构建者共享交易。
          
          参见：<https://docs.flashbots.net/flashbots-protect/quick-start#faster-transactions>

      --jwt-secret <JWT_SECRET>
          RPC 端点的JWT密钥。
          
          JWT密钥将用于创建RPC的JWT。例如，可以使用以下命令模拟CL `engine_forkchoiceUpdated`调用：
          
          cast rpc --jwt-secret <JWT_SECRET> engine_forkchoiceUpdatedV2
          '["0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc"]'
          
          [env: ETH_RPC_JWT_SECRET=]

  -h, --help
          打印帮助信息（使用'-h'查看摘要）
```
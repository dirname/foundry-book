```markdown
# cast block

获取区块信息

```bash
$ cast block --help
Usage: cast block [OPTIONS] [BLOCK]

Arguments:
  [BLOCK]
          查询的区块高度。
          
          也可以是标签 earliest, finalized, safe, latest, 或 pending。

Options:
  -f, --field <FIELD>
          如果指定，仅获取区块的指定字段

      --full
          [env: CAST_FULL_BLOCK=]

  -r, --rpc-url <URL>
          RPC 端点
          
          [env: ETH_RPC_URL=]

      --flashbots
          使用 Flashbots RPC URL 并启用快速模式（<https://rpc.flashbots.net/fast>）。
          
          这将私密地与所有注册的构建者共享交易。
          
          参见：<https://docs.flashbots.net/flashbots-protect/quick-start#faster-transactions>

      --jwt-secret <JWT_SECRET>
          RPC 端点的 JWT 密钥。
          
          JWT 密钥将用于创建 RPC 的 JWT。例如，可以使用以下命令模拟 CL `engine_forkchoiceUpdated` 调用：
          
          cast rpc --jwt-secret <JWT_SECRET> engine_forkchoiceUpdatedV2
          '["0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc"]'
          
          [env: ETH_RPC_JWT_SECRET=]

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

显示选项:
  -j, --json
          以 JSON 格式打印区块
```
```
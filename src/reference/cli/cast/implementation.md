```json
{
  "translation": "# cast 实现"
}
```

```json
{
  "translation": "获取 EIP-1967 实现账户"
}
```

```json
{
  "translation": "```bash\n$ cast implementation --help\nUsage: cast implementation [OPTIONS] <WHO>\n\nArguments:\n  <WHO>\n          要获取 nonce 的地址\n\nOptions:\n  -B, --block <BLOCK>\n          查询的区块高度。\n          \n          也可以是以下标签：earliest, finalized, safe, latest, 或 pending。\n\n  -r, --rpc-url <URL>\n           RPC 端点\n          \n          [env: ETH_RPC_URL=]\n\n      --flashbots\n          使用 Flashbots RPC URL 并启用快速模式（<https://rpc.flashbots.net/fast>）。\n          \n          这将私密地与所有注册的构建者共享交易。\n          \n          参见：<https://docs.flashbots.net/flashbots-protect/quick-start#faster-transactions>\n\n      --jwt-secret <JWT_SECRET>\n          RPC 端点的 JWT 密钥。\n          \n          JWT 密钥将用于创建 RPC 的 JWT。例如，可以使用以下命令模拟 CL `engine_forkchoiceUpdated` 调用：\n          \n          cast rpc --jwt-secret <JWT_SECRET> engine_forkchoiceUpdatedV2\n          '[\"0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc\",\n          \"0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc\",\n          \"0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc\"]'\n          \n          [env: ETH_RPC_JWT_SECRET=]\n\n  -h, --help\n          打印帮助信息（使用 '-h' 查看摘要）\n```"
}
```
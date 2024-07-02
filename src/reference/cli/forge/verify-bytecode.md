```markdown
# forge verify-bytecode

验证已部署的字节码与其源代码

```bash
$ forge verify-bytecode --help
Usage: forge verify-bytecode [OPTIONS] <ADDRESS> <CONTRACT> [ROOT]

Arguments:
  <ADDRESS>
          要验证的合约地址

  <CONTRACT>
          合约标识符，格式为 `<路径>:<合约名称>`

  [ROOT]
          项目根目录的路径

Options:
      --block <BLOCK>
          应验证字节码的区块

      --constructor-args <ARGS>
          用于生成创建代码的构造函数参数
          
          [aliases: encoded-constructor-args]

      --constructor-args-path <PATH>
          包含构造函数参数的文件路径

  -r, --rpc-url <RPC_URL>
          用于验证的 RPC URL
          
          [env: ETH_RPC_URL=]

      --verification-type <TYPE>
          验证类型：`full` 或 `partial`。参考：
          <https://docs.sourcify.dev/docs/full-vs-partial-match/>
          
          [default: full]
          [possible values: full, partial]

  -e, --etherscan-api-key <KEY>
          Etherscan（或等效）API 密钥
          
          [env: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [env: CHAIN=]

      --skip <SKIP>...
          跳过构建文件名包含给定过滤器的文件。
          
          `test` 和 `script` 是 `.t.sol` 和 `.s.sol` 的别名。

      --json
          抑制日志并将 JSON 结果输出到标准输出

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
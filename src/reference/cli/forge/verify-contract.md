# forge verify-contract

在 Etherscan 上验证智能合约

```bash
$ forge verify-contract --help
Usage: forge verify-contract [OPTIONS] <ADDRESS> [CONTRACT]

Arguments:
  <ADDRESS>
          要验证的合约地址

  [CONTRACT]
          合约标识符，格式为 `<路径>:<合约名称>`

Options:
      --constructor-args <ARGS>
          构造函数参数的 ABI 编码
          
          [aliases: encoded-constructor-args]

      --constructor-args-path <PATH>
          包含构造函数参数的文件路径

      --guess-constructor-args
          尝试从链上创建代码中提取构造函数参数

      --compiler-version <VERSION>
          用于构建智能合约的 `solc` 版本

      --num-of-optimizations <NUM>
          用于构建智能合约的优化运行次数
          
          [aliases: optimizer-runs]

      --flatten
          在验证前展平源代码

  -f, --force
          在验证前不编译展平的智能合约（如果传递了 --flatten）

      --skip-is-verified-check
          在验证前不检查合约是否已验证

      --watch
          提交后等待验证结果

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

      --show-standard-json-input
          打印标准 json 编译器输入。
          
          标准 json 编译器输入可以用于在浏览器中手动提交合约验证。

      --via-ir
          使用 Yul 中间表示编译管道

      --evm-version <EVM_VERSION>
          EVM 版本。
          
          覆盖配置中指定的版本。

  -e, --etherscan-api-key <KEY>
          Etherscan（或等效）API 密钥
          
          [env: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [env: CHAIN=]

  -r, --rpc-url <URL>
          RPC 端点
          
          [env: ETH_RPC_URL=]

      --flashbots
          使用 Flashbots RPC URL 并启用快速模式（<https://rpc.flashbots.net/fast>）。
          
          这会私下共享交易给所有注册的构建者。
          
          参见：<https://docs.flashbots.net/flashbots-protect/quick-start#faster-transactions>

      --jwt-secret <JWT_SECRET>
          RPC 端点的 JWT 密钥。
          
          JWT 密钥将用于创建 RPC 的 JWT。例如，可以使用以下命令模拟 CL `engine_forkchoiceUpdated` 调用：
          
          cast rpc --jwt-secret <JWT_SECRET> engine_forkchoiceUpdatedV2
          '["0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc",
          "0x6bb38c26db65749ab6e472080a3d20a2f35776494e72016d1e339593f21c59bc"]'
          
          [env: ETH_RPC_JWT_SECRET=]

      --retries <RETRIES>
          重试验证的次数
          
          [default: 5]

      --delay <DELAY>
          在验证尝试之间应用的可选延迟，以秒为单位
          
          [default: 5]

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Linker options:
      --libraries <LIBRARIES>
          设置预链接的库
          
          [env: DAPP_LIBRARIES=]

Verifier options:
      --verifier <VERIFIER>
          使用的合约验证提供者
          
          [default: etherscan]
          [possible values: etherscan, sourcify, blockscout, oklink]

      --verifier-url <VERIFIER_URL>
          自定义提供者的验证 URL
          
          [env: VERIFIER_URL=]
```
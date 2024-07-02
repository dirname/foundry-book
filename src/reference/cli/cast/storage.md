```markdown
# cast storage

获取合约存储槽的原始值

```bash
$ cast storage --help
Usage: cast storage [OPTIONS] <ADDRESS> [SLOT]

Arguments:
  <ADDRESS>
          合约地址

  [SLOT]
          存储槽编号

Options:
  -b, --block <BLOCK>
          查询的区块高度。
          
          也可以是 earliest、finalized、safe、latest 或 pending 等标签。

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

  -e, --etherscan-api-key <KEY>
          Etherscan（或类似）API 密钥
          
          [env: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或 EIP-155 链 ID
          
          [env: CHAIN=]

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Cache options:
      --force
          清除缓存和 artifacts 文件夹并重新编译

Build options:
      --no-cache
          禁用缓存

      --skip <SKIP>...
          跳过构建包含给定过滤器的文件。
          
          `test` 和 `script` 是 `.t.sol` 和 `.s.sol` 的别名。

Linker options:
      --libraries <LIBRARIES>
          设置预链接的库
          
          [env: DAPP_LIBRARIES=]

Compiler options:
      --ignored-error-codes <ERROR_CODES>
          忽略 solc 警告的错误代码

      --deny-warnings
          警告将触发编译器错误

      --no-auto-detect
          不自动检测 `solc` 版本

      --use <SOLC_VERSION>
          指定用于构建的 solc 版本或本地 solc 路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不访问网络。
          
          缺失的 solc 版本将不会被安装。

      --via-ir
          使用 Yul 中间表示编译管道

      --no-metadata
          不在字节码中附加任何元数据。
          
          这等同于将 `bytecode_hash` 设置为 `none` 并将 `cbor_metadata` 设置为 `false`。

      --silent
          启动时不打印任何内容

      --ast
          在编译器输出中包含 AST 的 JSON

      --evm-version <VERSION>
          目标 EVM 版本

      --optimize
          激活 Solidity 优化器

      --optimizer-runs <RUNS>
          优化器运行次数

      --extra-output <SELECTOR>...
          在合约的 artifact 中包含的额外输出。
          
          示例键：evm.assembly、ewasm、ir、irOptimized、metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值：metadata、ir、irOptimized、ewasm、evm.assembly

Project options:
  -o, --out <PATH>
          合约 artifacts 文件夹的路径

      --revert-strings <REVERT>
          回退字符串配置。
          
          可能的值为 "default"、"strip"（移除）、"debug"（Solidity 生成的回退字符串）和 "verboseDebug"

      --build-info
          生成构建信息文件

      --build-info-path <PATH>
          构建信息文件的输出路径目录

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在 Git 仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源目录

  -R, --remappings <REMAPPINGS>
          项目的 remappings

      --remappings-env <ENV>
          从环境获取项目的 remappings

      --cache-path <PATH>
          编译器缓存的路径

      --lib-paths <PATH>
          库文件夹的路径

      --hardhat
          使用 Hardhat 风格的项目布局。
          
          这等同于使用：`--contracts contracts --lib-paths node_modules`。
          
          [aliases: hh]

      --config-path <FILE>
          配置文件的路径
```
```
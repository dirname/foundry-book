```markdown
# forge create

部署智能合约

```bash
$ forge create --help
Usage: forge create [OPTIONS] <CONTRACT>

Arguments:
  <CONTRACT>
          合约标识符，格式为 `<路径>:<合约名称>`

Options:
      --constructor-args <ARGS>...
          构造函数参数

      --constructor-args-path <PATH>
          包含构造函数参数的文件路径

      --verify
          创建后验证合约

      --unlocked
          通过 `eth_sendTransaction` 使用 `--from` 参数或 `$ETH_FROM` 作为发送者

      --show-standard-json-input
          如果提供了 `--verify`，则打印标准 json 编译器输入。
          
          标准 json 编译器输入可以用于在浏览器中手动提交合约验证。

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

Display options:
      --json
          以 JSON 格式打印部署信息

Cache options:
      --force
          清除缓存和 artifacts 文件夹并重新编译

Build options:
      --no-cache
          禁用缓存

      --skip <SKIP>...
          跳过构建文件名包含给定过滤器的文件。
          
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
          指定用于构建的 solc 版本或本地 solc 的路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不访问网络。
          
          缺失的 solc 版本将不会被安装。

      --via-ir
          使用 Yul 中间表示编译管道

      --no-metadata
          不在字节码中附加任何元数据。
          
          这相当于将 `bytecode_hash` 设置为 `none` 并将 `cbor_metadata` 设置为 `false`。

      --silent
          启动时不打印任何内容

      --ast
          在编译器输出中包含 AST 作为 JSON

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
          构建信息文件将写入的目录输出路径

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源目录

  -R, --remappings <REMAPPINGS>
          项目的 remappings

      --remappings-env <ENV>
          从环境获取的项目的 remappings

      --cache-path <PATH>
          编译器缓存的路径

      --lib-paths <PATH>
          库文件夹的路径

      --hardhat
          使用 Hardhat 风格的
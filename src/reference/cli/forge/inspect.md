```markdown
# forge inspect

获取智能合约的专门信息

```bash
$ forge inspect --help
Usage: forge inspect [OPTIONS] <CONTRACT> <FIELD>

Arguments:
  <CONTRACT>
          要检查的合约标识符，格式为 `(<路径>:)?<合约名称>`

  <FIELD>
          要检查的合约构件字段
          
          [可能的值: abi, bytecode, deployedBytecode, assembly, assemblyOptimized,
          methodIdentifiers, gasEstimates, storageLayout, devdoc, ir, irOptimized, metadata,
          userdoc, ewasm, errors, events]

Options:
      --pretty
          如果支持，以漂亮的格式打印选定的字段

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

缓存选项:
      --force
          清除缓存和构件文件夹并重新编译

      --no-cache
          禁用缓存

      --skip <SKIP>...
          跳过构建名称包含给定过滤器的文件。
          
          `test` 和 `script` 是 `.t.sol` 和 `.s.sol` 的别名。

链接器选项:
      --libraries <LIBRARIES>
          设置预链接的库
          
          [环境变量: DAPP_LIBRARIES=]

编译器选项:
      --ignored-error-codes <ERROR_CODES>
          忽略 solc 警告的错误代码

      --deny-warnings
          警告将触发编译器错误

      --no-auto-detect
          不要自动检测 `solc` 版本

      --use <SOLC_VERSION>
          指定用于构建的 solc 版本或本地 solc 的路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不要访问网络。
          
          缺失的 solc 版本将不会被安装。

      --via-ir
          使用 Yul 中间表示编译管道

      --no-metadata
          不要在字节码中附加任何元数据。
          
          这相当于将 `bytecode_hash` 设置为 `none` 并将 `cbor_metadata` 设置为 `false`。

      --silent
          启动时不要打印任何内容

      --ast
          在编译器输出中包含 AST 的 JSON

      --evm-version <VERSION>
          目标 EVM 版本

      --optimize
          激活 Solidity 优化器

      --optimizer-runs <RUNS>
          优化器运行的次数

      --extra-output <SELECTOR>...
          在合约的构件中包含额外的输出。
          
          示例键: evm.assembly, ewasm, ir, irOptimized, metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值: metadata, ir, irOptimized, ewasm, evm.assembly

项目选项:
  -o, --out <PATH>
          合约构件文件夹的路径

      --revert-strings <REVERT>
          回退字符串配置。
          
          可能的值为 "default"、"strip"（移除）、"debug"（Solidity 生成的回退字符串）和 "verboseDebug"

      --build-info
          生成构建信息文件

      --build-info-path <PATH>
          构建信息文件将写入的目录输出路径

      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在 Git 仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源目录

  -R, --remappings <REMAPPINGS>
          项目的重映射

      --remappings-env <ENV>
          从环境变量中获取项目的重映射

      --cache-path <PATH>
          编译器缓存的路径

      --lib-paths <PATH>
          库文件夹的路径

      --hardhat
          使用 Hardhat 风格的项目布局。
          
          这相当于使用: `--contracts contracts --lib-paths node_modules`。
          
          [别名: hh]

      --config-path <FILE>
          配置文件的路径
```
```
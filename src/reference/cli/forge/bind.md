# forge bind

生成智能合约的 Rust 绑定

```bash
$ forge bind --help
Usage: forge bind [OPTIONS]

Options:
  -b, --bindings-path <PATH>
          存储合约工件的路径

      --select <SELECT>
          仅生成名称匹配指定过滤器的合约的绑定

      --select-all
          显式生成所有合约的绑定
          
          默认情况下，所有以 `Test` 或 `Script` 结尾的合约会被排除。

      --crate-name <NAME>
          生成的 Rust crate 的名称。
          
          这应该是一个有效的 crates.io crate 名称，然而，此命令目前不会验证这一点。
          
          [默认值: foundry-contracts]

      --crate-version <VERSION>
          生成的 Rust crate 的版本。
          
          这应该是一个标准的 semver 版本字符串，然而，此命令目前不会验证这一点。
          
          [默认值: 0.1.0]

      --module
          将绑定生成为模块而不是 crate

      --overwrite
          覆盖现有的生成绑定。
          
          默认情况下，命令会检查绑定是否正确，然后退出。如果传递了 --overwrite，则会删除并覆盖绑定。

      --single-file
          将绑定生成为单个文件

      --skip-cargo-toml
          跳过 Cargo.toml 一致性检查

      --skip-build
          在生成绑定之前跳过运行 forge build

      --skip-extra-derives
          不在生成的绑定中添加任何额外的 derives

      --alloy
          为 `alloy` 库生成绑定，而不是 `ethers`

      --alloy-version <ALLOY_VERSION>
          指定 alloy 版本

      --ethers
          为 `ethers` 库生成绑定，而不是 `alloy`（默认，已弃用）

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

缓存选项:
      --force
          清除缓存和工件文件夹并重新编译

构建选项:
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
          不自动检测 `solc` 版本

      --use <SOLC_VERSION>
          指定用于构建的 solc 版本或本地 solc 的路径。
          
          有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

      --offline
          不访问网络。
          
          缺失的 solc 版本不会被安装。

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
          在合约的工件中包含的额外输出。
          
          示例键: evm.assembly, ewasm, ir, irOptimized, metadata
          
          完整描述请参见
          <https://docs.soliditylang.org/en/v0.8.13/using-the-compiler.html#input-description>

      --extra-output-files <SELECTOR>...
          写入单独文件的额外输出。
          
          有效值: metadata, ir, irOptimized, ewasm, evm.assembly

项目选项:
  -o, --out <PATH>
          合约工件文件夹的路径

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
          项目的重映射

      --remappings-env <ENV>
          从环境获取项目的重映射

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
# forge

构建、测试、模糊测试、调试和部署 Solidity 合约

```bash
$ forge --help
Usage: forge <COMMAND>

Commands:
  bind               生成智能合约的 Rust 绑定
  build              构建项目的智能合约 [别名: b, compile]
  cache              管理 Foundry 缓存
  clean              删除构建产物和缓存目录 [别名: cl]
  clone              从 Etherscan 克隆合约
  completions        生成 shell 自动补全脚本 [别名: com]
  config             显示当前配置 [别名: co]
  coverage           生成覆盖率报告
  create             部署智能合约 [别名: c]
  debug              将单个智能合约作为脚本进行调试 [别名: d]
  doc                为项目生成文档
  flatten            将源文件及其所有导入合并到一个文件中 [别名: f]
  fmt                格式化 Solidity 源文件
  geiger             检测项目及其依赖项中不安全作弊代码的使用
  generate           生成脚手架文件
  generate-fig-spec  生成 Fig 自动补全规范 [别名: fig]
  help               打印此消息或给定子命令的帮助信息
  init               创建一个新的 Forge 项目
  inspect            获取有关智能合约的专业信息 [别名: in]
  install            安装一个或多个依赖项 [别名: i]
  remappings         获取项目自动推断的重映射 [别名: re]
  remove             删除一个或多个依赖项 [别名: rm]
  script             将智能合约作为脚本运行，构建可以上链发送的交易
  selectors          函数选择器工具 [别名: se]
  snapshot           创建每个测试的 gas 使用情况快照 [别名: s]
  test               运行项目的测试 [别名: t]
  tree               显示项目依赖图的树形可视化 [别名: tr]
  update             更新一个或多个依赖项 [别名: u]
  verify-bytecode    验证已部署字节码与其源码 [别名: vb]
  verify-check       在 Etherscan 上检查验证状态 [别名: vc]
  verify-contract    在 Etherscan 上验证智能合约 [别名: v]

Options:
  -h, --help     打印帮助信息
  -V, --version  打印版本信息

在书中找到更多信息: http://book.getfoundry.sh/reference/forge/forge.html
```
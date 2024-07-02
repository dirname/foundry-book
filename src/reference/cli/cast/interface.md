# cast 接口

从给定的 ABI 生成一个 Solidity 接口。

```bash
$ cast interface --help
用法: cast interface [选项] <路径或地址>

参数:
  <路径或地址>
          合约地址或 ABI 文件的路径。
          
          如果指定地址，则从 Etherscan 获取 ABI。

选项:
  -n, --name <名称>
          用于生成的接口的名称

  -p, --pragma <版本>
          Solidity 编译器版本
          
          [默认值: ^0.8.4]

  -o, --output <路径>
          输出文件的路径。
          
          如果未指定，接口将输出到标准输出。

  -j, --json
          如果指定，接口将以 JSON 格式输出，而不是 Solidity 格式

  -e, --etherscan-api-key <密钥>
          Etherscan（或类似）API 密钥
          
          [环境变量: ETHERSCAN_API_KEY=]

  -c, --chain <链>
          链名称或 EIP-155 链 ID
          
          [环境变量: CHAIN=]

  -h, --help
          打印帮助信息（使用 '-h' 查看简要帮助）
```
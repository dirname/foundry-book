```markdown
# forge clone

从Etherscan克隆一个合约

```bash
$ forge clone --help
Usage: forge clone [OPTIONS] <ADDRESS> [PATH]

Arguments:
  <ADDRESS>
          要克隆的合约地址

  [PATH]
          克隆项目的根目录
          
          [默认值: .]

Options:
      --no-remappings-txt
          不生成remappings.txt文件。而是将remappings保留在配置中

      --keep-directory-structure
          保留从Etherscan收集的原始目录结构。
          
          如果设置了此标志，克隆项目的目录结构将保持不变。默认情况下，目录结构会重新组织以提高可读性，但可能会导致某些编译失败。

  -e, --etherscan-api-key <KEY>
          Etherscan（或等效）API密钥
          
          [环境变量: ETHERSCAN_API_KEY=]

  -c, --chain <CHAIN>
          链名称或EIP-155链ID
          
          [环境变量: CHAIN=]

      --shallow
          执行浅克隆而不是深克隆。
          
          提高性能并减少磁盘使用，但阻止切换分支或标签。

      --no-git
          安装时不将依赖项作为子模块添加

      --no-commit
          不创建提交

  -q, --quiet
          不打印任何消息

  -h, --help
          打印帮助信息（使用'-h'查看摘要）
```
```
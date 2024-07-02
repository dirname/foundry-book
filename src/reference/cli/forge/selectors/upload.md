```markdown
# forge selectors upload

上传选择器到注册表

```bash
$ forge selectors upload --help
Usage: forge selectors upload [OPTIONS] [CONTRACT]

Arguments:
  [CONTRACT]
          要上传选择器的合约名称

Options:
      --all
          上传项目中所有合约的选择器

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

项目选项:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源代码目录

  -R, --remappings <REMAPPINGS>
          项目的重映射

      --remappings-env <ENV>
          从环境变量中获取项目的重映射

      --cache-path <PATH>
          编译器缓存路径

      --lib-paths <PATH>
          库文件夹路径

      --hardhat
          使用 Hardhat 风格的项目布局。
          
          这等同于使用：`--contracts contracts --lib-paths node_modules`。
          
          [aliases: hh]

      --config-path <FILE>
          配置文件路径
```
```
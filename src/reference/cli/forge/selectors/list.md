```markdown
# forge selectors list

列出当前工作区的选择器

```bash
$ forge selectors list --help
Usage: forge selectors list [OPTIONS] [CONTRACT]

Arguments:
  [CONTRACT]
          要列出选择器的合约名称。

Options:
  -h, --help
          打印帮助信息（使用 '-h' 查看简要帮助）

项目选项:
      --root <PATH>
          项目的根路径。
          
          默认情况下，如果在一个 Git 仓库中，则为 Git 仓库的根路径，否则为当前工作目录。

  -C, --contracts <PATH>
          合约源代码目录

  -R, --remappings <REMAPPINGS>
          项目的重映射

      --remappings-env <ENV>
          从环境变量中获取的项目重映射

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
```markdown
# forge tree

显示项目的依赖关系图的树状可视化

```bash
$ forge tree --help
Usage: forge tree [OPTIONS]

Options:
      --no-dedupe
          不要去重（重复所有共享依赖）

      --charset <CHARSET>
          在输出中使用的字符集。
          
          [可能的值: utf8, ascii]
          
          [默认值: utf8]

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

项目选项:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在仓库中），或者当前工作目录。

  -C, --contracts <PATH>
          合约源代码目录

  -R, --remappings <REMAPPINGS>
          项目的重映射

      --remappings-env <ENV>
          从环境获取的项目重映射

      --cache-path <PATH>
          编译器缓存路径

      --lib-paths <PATH>
          库文件夹路径

      --hardhat
          使用 Hardhat 风格的项目布局。
          
          这等同于使用: `--contracts contracts --lib-paths node_modules`。
          
          [别名: hh]

      --config-path <FILE>
          配置文件路径
```
```
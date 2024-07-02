```markdown
# forge flatten

将源文件及其所有导入合并到一个文件中

```bash
$ forge flatten --help
Usage: forge flatten [OPTIONS] <PATH>

Arguments:
  <PATH>
          要合并的合约路径

Options:
  -o, --output <PATH>
          输出合并后合约的路径。
          
          如果未指定，合并后的合约将输出到标准输出。

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）

项目选项:
      --root <PATH>
          项目的根路径。
          
          默认情况下，如果在一个 Git 仓库中，则为 Git 仓库的根目录，否则为当前工作目录。

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
          
          这等同于使用：`--contracts contracts --lib-paths node_modules`。
          
          [aliases: hh]

      --config-path <FILE>
          配置文件路径
```
```
# forge geiger

检测项目及其依赖项中使用的不安全作弊代码

```bash
$ forge geiger --help
Usage: forge geiger [OPTIONS] [PATH]...

Arguments:
  [PATH]...
          要检测的文件或目录路径

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

      --check
          以“检查”模式运行。
          
          程序的退出代码将是找到的不安全作弊代码的数量。

      --ignore <PATH>...
          要忽略的通配符

      --full
          打印所有文件的报告，即使没有找到不安全函数

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
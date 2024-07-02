```markdown
# forge fmt

格式化 Solidity 源文件

```bash
$ forge fmt --help
Usage: forge fmt [OPTIONS] [PATH]...

Arguments:
  [PATH]...
          文件路径、目录或 '-' 从标准输入读取

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），否则是当前工作目录。

      --check
          以 'check' 模式运行。
          
          如果输入格式正确则退出代码为 0。如果需要格式化则退出代码为 1。

  -r, --raw
          在 'check' 和标准输入模式下，输出原始格式化代码而不是差异

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
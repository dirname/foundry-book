```markdown
# forge update

更新一个或多个依赖项。

```bash
$ forge update --help
Usage: forge update [OPTIONS] [DEPENDENCIES]...

Arguments:
  [DEPENDENCIES]...
          你想要更新的依赖项

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

  -f, --force
          覆盖最新的检查

  -r, --recursive
          递归更新子模块

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
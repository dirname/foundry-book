```markdown
# forge remove

移除一个或多个依赖项

```bash
$ forge remove --help
Usage: forge remove [OPTIONS] <DEPENDENCIES>...

Arguments:
  <DEPENDENCIES>...
          你想要移除的依赖项

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根路径（如果在 Git 仓库中），或者当前工作目录。

  -f, --force
          覆盖最新的检查

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
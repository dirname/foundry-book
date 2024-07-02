```markdown
# forge install

安装一个或多个依赖项。

```bash
$ forge install --help
Usage: forge install [OPTIONS] [DEPENDENCIES]...
    forge install [OPTIONS] <github 用户名>/<github 项目>@<标签>...
    forge install [OPTIONS] <别名>=<github 用户名>/<github 项目>@<标签>...
    forge install [OPTIONS] <https:// git 地址>...

Arguments:
  [DEPENDENCIES]...
          要安装的依赖项。
          
          依赖项可以是一个原始 URL，或 GitHub 仓库的路径。
          
          此外，可以通过在依赖路径后添加 @ 来提供 ref。
          
          ref 可以是：
          - 分支：master
          - 标签：v1.2.3
          - 提交：8e8128
          
          可以通过 `<别名>=` 后缀添加目标安装目录。依赖项将安装到 `lib/<别名>`。

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下是 Git 仓库的根目录（如果在 Git 仓库中），或当前工作目录。

      --shallow
          执行浅克隆而不是深克隆。
          
          提高性能并减少磁盘使用，但阻止切换分支或标签。

      --no-git
          安装时不将依赖项添加为子模块

      --no-commit
          不创建提交

  -q, --quiet
          不打印任何消息

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
```markdown
# forge init

创建一个新的 Forge 项目

```bash
$ forge init --help
Usage: forge init [OPTIONS] [PATH]

Arguments:
  [PATH]
          新项目的根目录
          
          [默认值: .]

Options:
  -t, --template <TEMPLATE>
          从模板开始

  -b, --branch <BRANCH>
          只能与模板选项一起使用的分支参数。如果未指定，则使用默认分支

      --offline
          不要从网络安装依赖项
          
          [别名: no-deps]

      --force
          即使指定的根目录不为空，也要创建项目

      --vscode
          创建一个包含 Solidity 设置的 .vscode/settings.json 文件，并生成一个 remappings.txt 文件

      --shallow
          执行浅克隆而不是深克隆。
          
          提高性能并减少磁盘使用，但阻止切换分支或标签。

      --no-git
          安装时不将依赖项作为子模块添加

      --no-commit
          不要创建提交

  -q, --quiet
          不打印任何消息

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
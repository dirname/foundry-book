```markdown
# forge doc

生成项目的文档

```bash
$ forge doc --help
Usage: forge doc [OPTIONS]

Options:
      --root <PATH>
          项目的根路径。
          
          默认情况下，如果是 Git 仓库，则为仓库的根目录，否则为当前工作目录。

  -o, --out <PATH>
          文档的输出路径。
          
          默认情况下，它是项目根目录下的 `docs/` 目录。

  -b, --build
          从生成的文件构建 `mdbook`

  -s, --serve
          提供文档服务

      --open
          在浏览器中打开文档服务后

      --hostname <HOSTNAME>
          提供文档服务的主机名

  -p, --port <PORT>
          提供文档服务的端口

      --deployments [<DEPLOYMENTS>]
          到 `hardhat-deploy` 或 `forge-deploy` 工件目录的相对路径。留空为默认值

  -i, --include-libraries
          是否为外部库创建文档

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
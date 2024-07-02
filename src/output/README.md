## CLI 输出

**这是由 [`scripts/gen_output.sh`](../../scripts/gen_output.sh) 自动生成的内容**，使用了仓库根目录中的 [项目](../../projects)。

如果 `forge` 或 `cast` 的 CLI 输出发生了重大变化，或者你对项目进行了更改，那么你应该从仓库根目录运行 `./scripts/gen_output.sh`。

确保你已经通过运行 `git submodule update --init --recursive` 拉取了所有子模块。

要在此处使用输出，请参考包含此文件夹中 CLI 输出的现有位置，或查看 [mdbook 的文档](https://rust-lang.github.io/mdBook/format/mdbook.html) 以获取更多信息。

### 为什么？

这有一些好处：

- 我们检查我们使用的代码片段是否实际编译
- 我们检查书中的命令是否实际运行
- 如果输出看起来不正确，我们可能已经检测到一个错误
- 有时 `cast` 和 `forge` 的输出变化非常快 - 在这些情况下，运行一个命令比手动更改每个页面更容易
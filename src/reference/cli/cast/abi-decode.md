```markdown
# cast abi-decode

解码 ABI 编码的输入或输出数据。

```bash
$ cast abi-decode --help
Usage: cast abi-decode [OPTIONS] <SIG> <CALLDATA>

Arguments:
  <SIG>
          函数签名，格式为 `<name>(<in-types>)(<out-types>)`

  <CALLDATA>
          ABI 编码的 calldata

Options:
  -h, --help
          打印帮助信息（使用 '-h' 查看简要帮助）

解码输入数据而不是输出数据：
  -i, --input
          是否解码输入数据或输出数据
```
```
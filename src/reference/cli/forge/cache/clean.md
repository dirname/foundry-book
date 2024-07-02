```markdown
# forge cache clean

清除全局foundry目录中的缓存数据

```bash
$ forge cache clean --help
Usage: forge cache clean [OPTIONS] [CHAINS]...

Arguments:
  [CHAINS]...
          要清除缓存的链。
          
          也可以是 "all" 以清除所有链的缓存。
          
          [env: CHAIN=]
          [default: all]

Options:
  -b, --blocks <BLOCKS>...
          要清除缓存的区块

      --etherscan
          是否清除Etherscan缓存

  -h, --help
          打印帮助信息（使用 '-h' 查看摘要）
```
```
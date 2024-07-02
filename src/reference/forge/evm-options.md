```json
{
  "title": "EVM 选项",
  "options": [
    {
      "name": "-f",
      "alias": ["--rpc-url", "--fork-url"],
      "description": "从远程端点获取状态，而不是从空状态开始。",
      "note": "如果要从特定区块号获取状态，请参见 `--fork-block-number`。"
    },
    {
      "name": "--fork-block-number",
      "description": "从远程端点获取特定区块号的状态。请参见 `--fork-url`。"
    },
    {
      "name": "--fork-retry-backoff",
      "description": "遇到错误时的初始重试退避时间。"
    },
    {
      "name": "--no-storage-caching",
      "description": "显式禁用 RPC 缓存的使用。",
      "note": "所有存储槽完全从端点读取。请参见 `--fork-url`。"
    },
    {
      "name": "-v",
      "alias": "--verbosity",
      "description": "EVM 的详细程度。",
      "note": "多次传递以增加详细程度（例如 `-v`, `-vv`, `-vvv`）。",
      "levels": [
        {
          "level": 2,
          "description": "打印所有测试的日志"
        },
        {
          "level": 3,
          "description": "打印失败测试的执行 traces"
        },
        {
          "level": 4,
          "description": "打印所有测试的执行 traces，以及失败测试的设置 traces"
        },
        {
          "level": 5,
          "description": "打印所有测试的执行和设置 traces"
        }
      ]
    },
    {
      "name": "--sender",
      "description": "将执行测试的地址"
    },
    {
      "name": "--initial-balance",
      "description": "已部署合约的初始余额"
    },
    {
      "name": "--ffi",
      "description": "启用 [FFI cheatcode][ffi-cheatcode]"
    }
  ],
  "links": {
    "ffi-cheatcode": "../../cheatcodes/ffi.md"
  }
}
```
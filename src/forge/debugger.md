## 调试器

Forge 附带了一个交互式调试器。

调试器可以通过 [`forge debug`](../reference/forge/forge-debug.md) 和 [`forge test`](../reference/forge/forge-test.md) 访问。

使用 `forge test`：

```sh
$ forge test --debug $FUNC
```

其中 `$FUNC` 是你想要调试的函数的签名。例如：

```sh
$ forge test --debug "testSomething()"
```

如果你有多个合约具有相同函数名，你需要使用 `--match-path` 和 `--match-contract` 将匹配的函数限制为仅一个。

如果匹配的测试是一个模糊测试，调试器将打开第一个失败的模糊场景，或者最后一个成功的场景，以先到者为准。

使用 `forge debug`：

```sh
$ forge debug --debug $FILE --sig $FUNC
```

其中 `$FILE` 是你想要调试的合约的路径，`$FUNC` 是你想要调试的函数的签名。例如：

```sh
$ forge debug --debug src/SomeContract.sol --sig "myFunc(uint256,string)" 123 "hello"
```

你也可以使用 `--sig` 指定原始调用数据，而不是函数签名。

如果你的源文件包含多个合约，使用 `--target-contract` 标志指定你想要调试的合约。

### 调试器布局

![调试器 UI 的图像](../images/debugger.png)

当调试器运行时，你会看到一个分为四个象限的终端：

- **象限 1**：调试会话中的操作码，当前操作码高亮显示。此外，还会显示当前账户的地址、程序计数器和累积的 gas 使用量。
- **象限 2**：当前堆栈，以及堆栈的大小。
- **象限 3**：源代码视图。
- **象限 4**：EVM 的当前内存。

当你逐步执行代码时，你会注意到堆栈和内存中的单词有时会改变颜色。

对于内存：

- **红色单词** 是当前操作码即将写入的。
- **绿色单词** 是前一个操作码写入的。
- **青色单词** 是当前操作码正在读取的。

对于堆栈，**青色单词** 是当前操作码正在读取或弹出的。

> ⚠️ **注意**
>
> 在大多数测试框架中，第一个失败的测试断言是报告的那个。
> 在 foundry 中，最后一个失败的测试断言（来自 DSTest 或 cheatcodes）是报告的那个。

### 导航

### 通用

- <kbd>q</kbd>：退出调试器
- <kbd>h</kbd>：显示帮助

### 导航调用

- <kbd>0-9</kbd> + <kbd>k</kbd>：向后步进一定次数（或者用鼠标向上滚动）
- <kbd>0-9</kbd> + <kbd>j</kbd>：向前步进一定次数（或者用鼠标向下滚动）
- <kbd>g</kbd>：移动到交易的开头
- <kbd>G</kbd>：移动到交易的结尾
- <kbd>c</kbd>：移动到前一个调用类型的指令（即 [`CALL`][op-call]、[`STATICCALL`][op-staticcall]、[`DELEGATECALL`][op-delegatecall] 和 [`CALLCODE`][op-callcode]）。
- <kbd>C</kbd>：移动到下一个调用类型的指令
- <kbd>a</kbd>：移动到前一个 [`JUMP`][op-jump] 或 [`JUMPI`][op-jumpi] 指令
- <kbd>s</kbd>：移动到下一个 [`JUMPDEST`][op-jumpdest] 指令
- <kbd>'</kbd> + <kbd>a-z</kbd>：移动到由 [`vm.breakpoint`][cheat-breakpoint] cheatcode 设置的 `<char>` 断点

### 导航内存

- <kbd>Ctrl</kbd> + <kbd>j</kbd>：向下滚动内存视图
- <kbd>Ctrl</kbd> + <kbd>k</kbd>：向上滚动内存视图
- <kbd>m</kbd>：以 UTF8 格式显示内存

### 导航堆栈

- <kbd>J</kbd>：向下滚动堆栈视图
- <kbd>K</kbd>：向上滚动堆栈视图
- <kbd>t</kbd>：在堆栈上显示标签，以查看当前操作码将消耗哪些项目

[op-call]: https://www.evm.codes/#f1
[op-staticcall]: https://www.evm.codes/#fa
[op-delegatecall]: https://www.evm.codes/#f4
[op-callcode]: https://www.evm.codes/#f2
[op-jumpdest]: https://www.evm.codes/#5b
[op-jump]: https://www.evm.codes/#f1
[op-jumpi]: https://www.evm.codes/#57
[cheat-breakpoint]: ../cheatcodes/breakpoint.md
## 常见问题解答

这是一个常见问题和答案的集合。如果你在这里没有找到你的问题，请加入 [Telegram 支持频道][tg-support]，让我们帮助你！

### 我无法从源码构建！

确保你使用的是最新的稳定 Rust 工具链：
```sh
rustup default stable
rustup update stable
```

### 运行 `forge`/`cast` 时出现 `libusb` 错误

如果你使用的是发布的二进制文件，在 MacOS 上可能会看到以下错误：

```sh
dyld: Library not loaded: /usr/local/opt/libusb/lib/libusb-1.0.0.dylib
```

为了解决这个问题，你必须安装 `libusb` 库：

```sh
brew install libusb
```

### `GLIBC` 版本过旧

如果你在使用 `foundryup` 后遇到类似以下的错误：

```sh
forge: /lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.29' not found (required by forge)
```

有两个解决方法：

1. [从源码构建](./getting-started/installation.md#building-from-source)
2. [使用 Docker](./getting-started/installation.md#using-foundry-with-docker)

### 救命！我看不到我的日志！

Forge 默认不显示日志。如果你想查看 Hardhat 的 `console.log` 或 DSTest 风格的 `log_*` 事件日志，你需要运行 [`forge test`][forge-test] 并设置详细度为 2（`-vv`）。

如果你想查看其他合约发出的事件，你需要启用 traces。为此，将详细度设置为 3（`-vvv`）以查看失败的测试的 traces，或设置为 4（`-vvvv`）以查看所有测试的 traces。

### 我的测试失败了，我不知道为什么！

为了更好地了解你的测试为何失败，尝试使用 traces。要启用 traces，你需要在 [forge test][forge-test] 上将详细度至少增加到 3（`-vvv`），但你可以增加到 5（`-vvvvv`）以获得更多的 traces。

你可以在我们的 [理解 Traces][traces] 章节中了解更多关于 traces 的信息。

### 如何使用 `console.log`？

要使用 Hardhat 的 `console.log`，你必须通过从 [这里][console-log] 复制文件来将其添加到你的项目中。

或者，你可以使用 [Forge Std][forge-std]，它自带 `console.log`。要使用 Forge Std 中的 `console.log`，你必须导入它：

```solidity
import "forge-std/console.sol";
```

### 如何运行特定的测试？

如果你只想运行几个测试，可以使用 `--match-test` 来过滤测试函数，使用 `--match-contract` 来过滤测试合约，以及使用 `--match-path` 来过滤测试文件在 [`forge test`][forge-test] 上。

### 如何使用特定的 Solidity 编译器？

Forge 会尝试自动检测适用于你项目的 Solidity 编译器。

要使用特定的 Solidity 编译器，你可以在配置文件中设置 [`solc`][config-solc]，或者在支持的 Forge 命令中传递 `--use solc:<version>`（例如 [`forge build`][forge-build] 或 [`forge test`][forge-test]）。也可以接受 solc 二进制文件的路径。要使用特定的本地 solc 二进制文件，可以在配置文件中设置 `solc = "<path to solc>"`，或者传递 `--use "<path to solc>"`。solc 版本/路径也可以通过环境变量 `FOUNDRY_SOLC=<version/path>` 设置，但命令行参数 `--use` 优先。

例如，如果你有一个支持所有 0.7.x Solidity 版本的项目，但你想用 solc 0.7.0 编译，你可以使用 `forge build --use solc:0.7.0`。

### 如何从实时网络分叉？

要从实时网络分叉，请在 [`forge test`][forge-test] 上传递 `--fork-url <URL>`。你还可以使用 `--fork-block-number <BLOCK>` 从特定区块分叉，这会增加测试的确定性，并允许 Forge 缓存该区块的链数据。

例如，要从以太坊主网在区块 10,000,000 分叉，你可以使用：`forge test --fork-url $MAINNET_RPC_URL --fork-block-number 10000000`。

### 如何添加我自己的断言？

你可以通过创建自己的基础测试合约并让该合约继承你选择的测试框架来添加自己的断言。

例如，如果你使用 DSTest，你可以创建一个基础测试合约如下：

```solidity
contract TestBase is DSTest {
    function myCustomAssertion(uint a, uint b) {
      if (a != b) {
          emit log_string("a 和 b 不匹配");
          fail();
      }
    }
}
```

然后在你
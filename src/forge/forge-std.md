## Forge 标准库概述

Forge 标准库（简称 Forge Std）是一组有用的合约，使编写测试更容易、更快、更用户友好。

使用 Forge Std 是使用 Foundry 编写测试的首选方式。

它提供了开始编写测试所需的所有基本功能：

- `Vm.sol`：最新的作弊码接口
- `console.sol` 和 `console2.sol`：Hardhat 风格的日志功能
- `Script.sol`：用于 [Solidity 脚本](../tutorials/solidity-scripting.md) 的基本实用程序
- `Test.sol`：包含标准库、作弊码实例（`vm`）和 Hardhat 控制台的 DSTest 超集

只需导入 `Test.sol` 并在测试合约中继承 `Test`：

```solidity
import "forge-std/Test.sol";

contract ContractTest is Test { ...
```

现在，你可以：

```solidity
// 通过 `vm` 实例访问 Hevm
vm.startPrank(alice);

// 使用 Dappsys Test 进行断言和日志
assertEq(dai.balanceOf(alice), 10000e18);

// 使用 Hardhat `console` (`console2`) 进行日志
console.log(alice.balance);

// 使用 Forge Std 标准库中的任何内容
deal(address(dai), alice, 10000e18);
```

要单独导入 `Vm` 接口或 `console` 库：

```solidity
import "forge-std/Vm.sol";
```

```solidity
import "forge-std/console.sol";
```

**注意：** `console2.sol` 包含对 `console.sol` 的补丁，允许 Forge 解码对控制台的调用跟踪，但它与 Hardhat 不兼容。

```solidity
import "forge-std/console2.sol";
```

### 标准库

Forge Std 目前由六个标准库组成。

#### Std Logs

Std Logs 扩展了 [`DSTest`](../reference/ds-test.md#logging) 库的日志事件。

#### Std Assertions

Std Assertions 扩展了 [`DSTest`](../reference/ds-test.md#asserting) 库的断言函数。

#### Std Cheats

Std Cheats 是围绕 Forge 作弊码的包装器，使它们更安全且改善了开发体验（DX）。

你可以在测试合约中简单地调用它们，就像调用任何其他内部函数一样：

```solidity
// 以 Alice 身份设置一个恶作剧，余额为 100 ETH
hoax(alice, 100 ether);
```

#### Std Errors

Std Errors 提供了围绕常见内部 Solidity 错误和回滚的包装器。

Std Errors 在与 [`expectRevert`](../cheatcodes/expect-revert.md) 作弊码结合使用时最有用，因为你不需要记住内部 Solidity 恐慌代码。注意，你必须通过 `stdError` 访问它们，因为这是一个库。

```solidity
// 在下一次调用时预期一个算术错误（例如下溢）
vm.expectRevert(stdError.arithmeticError);
```

#### Std Storage

Std Storage 使操作合约存储变得容易。它可以找到并与特定变量关联的存储槽进行读写。

`Test` 合约已经通过 `stdstore` 提供了一个 `StdStorage` 实例，你可以通过它访问任何 std-storage 功能。注意，你必须首先在你的测试合约中添加 `using stdStorage for StdStorage`。

```solidity
// 在合约 `game` 中找到变量 `score`
// 并将其值更改为 10
stdstore
    .target(address(game))
    .sig(game.score.selector)
    .checked_write(10);
```

#### Std Math

Std Math 是一个包含 Solidity 中未提供的实用数学函数的库。

注意，你必须通过 `stdMath` 访问它们，因为这是一个库。

```solidity
// 获取 -10 的绝对值
uint256 ten = stdMath.abs(-10)
```

<br>

> 📚 **参考**
>
> 请参阅 [Forge 标准库参考](../reference/forge-std/) 以获取 Forge 标准库的完整概述。
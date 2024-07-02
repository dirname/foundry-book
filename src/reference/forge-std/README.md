## Forge 标准库参考

Forge 标准库（简称 Forge Std）是一组有用的合约，使编写测试更容易、更快、更用户友好。

使用 Forge Std 是使用 Foundry 编写测试的首选方式。

包含的内容：

- `Vm.sol`：最新的 [作弊码接口](../../cheatcodes/#cheatcodes-interface)

    ```solidity
    import "forge-std/Vm.sol";
    ```

- [`console.sol`](./console-log.md) 和 `console2.sol`：Hardhat 风格的日志功能

    ```solidity
    import "forge-std/console.sol";
    ```

    **注意：** `console2.sol` 包含对 `console.sol` 的补丁，允许 Forge 解码对控制台的调用，但它与 Hardhat 不兼容。

    ```solidity
    import "forge-std/console2.sol";
    ```

- `Script.sol`：[Solidity 脚本](../../tutorials/solidity-scripting.md) 的基本实用工具

    ```solidity
    import "forge-std/Script.sol";
    ```

- `Test.sol`：完整的 Forge Std 体验（更多详情[如下](#forge-stds-test)）

    ```solidity
    import "forge-std/Test.sol";
    ```

### Forge Std 的 `Test`

`Test.sol` 中的 `Test` 合约提供了开始编写测试所需的所有基本功能。

只需导入 `Test.sol` 并在你的测试合约中继承 `Test`：

```solidity
import "forge-std/Test.sol";

contract ContractTest is Test { ...
```

包含的内容：

- 标准库
  - [Std Logs](./std-logs.md)：扩展 DSTest 库的日志事件。
  - [Std Assertions](./std-assertions.md)：扩展 DSTest 库的断言函数。
  - [Std Cheats](./std-cheats.md)：围绕 Forge 作弊码的包装器，以提高安全性和开发体验。
  - [Std Errors](./std-errors.md)：围绕常见内部 Solidity 错误和回滚的包装器。
  - [Std Storage](./std-storage.md)：存储操作的实用工具。
  - [Std Math](./std-math.md)：有用的数学函数。
  - [Script Utils](./script-utils.md)：可以在测试和脚本中访问的实用函数。
  - [Console Logging](./console-log.md)：控制台日志函数。

- 作弊码实例 `vm`，从中你可以调用 Forge 作弊码（见 [作弊码参考](../../cheatcodes/)）

    ```solidity
    vm.startPrank(alice);
    ```

- 所有 Hardhat `console` 函数用于日志记录（见 [Console Logging](./console-log.md)）

    ```solidity
    console.log(alice.balance); // 或 `console2`
    ```

- 所有 Dappsys Test 函数用于断言和日志记录（见 [Dappsys Test 参考](../ds-test.md)）

    ```solidity
    assertEq(dai.balanceOf(alice), 10000e18);
    ```

- `Script.sol` 中也包含的实用函数（见 [Script Utils](./script-utils.md)）

    ```solidity
    // 计算给定部署者地址和 nonce 的合约地址
    address futureContract = computeCreateAddress(alice, 1);
    ```
## 使用 CREATE2 进行确定性部署

### 介绍

作为 2019 年 [Constantinople 分叉](https://ethereum.org/en/history/#constantinople) 的一部分，`CREATE2` 操作码被纳入 EVM，其起源可以追溯到 [EIP-1014](https://eips.ethereum.org/EIPS/eip-1014)。
`CREATE2` 允许你根据部署者控制的参数将智能合约部署到确定性的地址。
因此，它经常被提及为启用“反事实”部署，你可以在尚未创建的地址上进行交互，因为 `CREATE2` 保证已知代码可以放置在该地址上。
这与 `CREATE` 操作码形成对比，后者的部署合约地址是部署者 nonce 的函数。
使用 `CREATE2`，你可以在多个网络上使用相同的部署者账户将合约部署到相同的地址，即使该地址的 nonce 不同。

> ℹ️ **注意**
> 本指南旨在帮助理解 `CREATE2`。在大多数用例中，你不需要编写和使用自己的部署者，可以使用现有的确定性部署者。在 forge 脚本中，使用 `new MyContract{salt: salt}()` 将使用位于 [0x4e59b44847b379578588920ca78fbf26c0b4956c](https://github.com/Arachnid/deterministic-deployment-proxy) 的确定性部署者。

在本教程中，我们将：

1. 查看一个 `CREATE2` 工厂实现。
2. 使用传统的部署方法部署工厂。
3. 使用这个已部署的工厂在确定性地址上部署一个简单的计数器合约。
4. 通过在 Foundry 中编写一个简单的测试来模拟这一系列事件。

### 前提条件

1. 需要对 Solidity 和 Foundry 有一定的了解，建议对内联汇编有一定的了解。
   参考 [官方 Solidity 文档](https://docs.soliditylang.org/en/latest/assembly.html) 了解内联汇编的基础知识。
2. 确保你的系统上已经 [安装](../getting-started/installation.md) 了 Foundry。
3. [初始化](../projects/creating-a-new-project.md) 一个新的 Foundry 项目。

### CREATE2 工厂

在 `src` 目录中创建一个名为 `Create2.sol` 的文件。
初始化一个名为 `Create2` 的合约，如下所示：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Create2 {

    error Create2EmptyBytecode();

    error Create2FailedDeployment();
}
```

这些错误旨在对工厂部署进行一些合理性检查，并在触发时回滚整个交易。
如果传递给 `deploy` 函数的字节码为空，则触发 `Create2EmptyBytecode()` 错误，如果部署因任何原因失败，则触发 `Create2FailedDeployment()` 错误。

> ℹ️ **注意**
>
> 请注意，`CREATE2` 部署可能由于多种原因失败。例如，如果字节码无效，或者在计算的地址上已经部署了合约。如果你的构造函数因任何原因回滚，你的部署也可能失败。

接下来，创建一个名为 `deploy` 的函数：

```solidity
function deploy(bytes32 salt, bytes memory creationCode) external payable returns (address addr) {
 
 }
```

该函数接受两个输入：

1. 用于计算最终地址的 `salt`。这基本上可以是任何我们想要的随机值。
2. 我们要部署的合约的创建代码。

新部署的合约地址在成功部署后返回。

> ℹ️ **注意**
>
> 你可以使用 `CREATE2` 向正在部署的合约发送 ETH，但前提是它有一个可支付的构造函数。
> 如果你尝试在没有可支付构造函数的情况下向其发送 ETH，交易将回滚。

在 deploy 函数的顶部添加这个回滚语句。我们希望如果以下条件不满足，函数拒绝部署请求：

```solidity
    if (creationCode.length == 0) {
        revert Create2EmptyBytecode();
    }
```

接下来，我们将使用内联汇编调用 `CREATE2` 操作码，这可以使用 `assembly` 关键字完成：

要调用 [内联汇编中的 CREATE2 操作码](https://docs.soliditylang.org/en/latest/yul.html#evm-dialect)，我们需要传入四个参数：

```solidity
    assembly {
        addr := create2(callvalue(), add(creationCode, 0x20), mload(creationCode), salt)
    }
```

1. 作为部署的一部分，我们要发送到新地址的 ETH 数量。这里我们传入 `callvalue()`，这是作为交易的一部分发送到工厂合约的 ETH 数量。可以将其视为 `msg.value` 的低级版本。
2. 第二个和第三个参数指的是我们的字节码所在的内存范围。`add(bytecode, 0x20)` 接受内存中 `bytes` 变量字节码位置的引用，并跳过 32 字节（十六进制的 0x20）以指向实际的字节码。

> ℹ️ **注意**
>
> Solidity 中的 `bytes` 类型是动态大小的字节数组，其中内存的前 32 字节表示数组的长度，其余字节表示实际数据。因此，当我们传入 `bytes` 变量的引用时，我们需要跳过前 32 字节以指向实际数据。
> 在 [evm.codes](https://www.evm.codes/?fork=shanghai) 上阅读更多关于 `add` 和 `mload` 操作码的信息。

最后，如果部署因任何原因失败，我们将回滚整个交易，在这种情况下，`CREATE2` 操作码将返回一个 0 地址：

```solidity
        if (addr == address(0)) {
            revert Create2FailedDeployment();
        }
```

最后，让我们创建一个名为 `computeAddress` 的视图函数。该函数应接受 `salt` 和 `creationCode` 作为参数，并返回使用 `deploy` 函数部署的合约地址：

```solidity
function computeAddress(bytes32 salt, bytes32 creationCodeHash) external view returns (address addr) {

 }
```

在函数内部，粘贴以下代码，该代码使用内联汇编计算地址，执行与 `CREATE2` 操作码相同的计算：

```solidity
    address contractAddress = address(this);
        
    assembly {
        let ptr := mload(0x40)

        mstore(add(ptr, 0x40), creationCodeHash)
        mstore(add(ptr, 0x20), salt)
        mstore(ptr, contractAddress)
        let start := add(ptr, 0x0b)
        mstore8(start, 0xff)
        addr := keccak256(start, 85)
    }
```

在尝试理解这里的汇编代码之前，让我们看一下 `CREATE2` 操作码用于计算地址的公式：

```bash
keccak256(0xff ++ address ++ salt ++ keccak256(bytecode))[12:]
```

`0xff` 是一个硬编码的前缀，防止使用 `CREATE` 和 `CREATE2` 部署的地址之间的哈希碰撞。
`address` 参数指的是调用 `CREATE2` 操作码的合约地址，在我们的例子中是工厂合约。
这四个参数连接在一起，并使用 `keccak256` 生成一个 32 字节的哈希。
前 12 字节被截断，剩下的 20 字节用作部署合约的地址。

`computeAddress` 函数中的整个汇编代码试图在不调用 `CREATE2` 操作码的情况下重现相同的公式：

1. `mload(0x40)` 将自由内存指针加载到内存中。这是指向内存数组中下一个自由内存槽的指针。在 [Solidity 文档](https://docs.soliditylang.org/en/latest/assembly.html#memory-management) 中阅读更多关于这方面的信息。
2. `mstore(add(ptr, 0x40), bytecodeHash)` 从 `ptr + 0x40` 指向的内存位置开始存储 `bytecodeHash`，即 `ptr + 64 字节`。
3. `mstore(add(ptr, 0x20), salt)` 在 `ptr + 0x20` 指向的内存位置存储 `salt`。
4. `mstore(ptr, contractAddress)` 在 `ptr` 指向的内存位置存储 `contractAddress`。

> ℹ️ **注意**
>
> 回想一下，传递给 `computeAddress` 函数的所有参数都是 32 字节长，并在内存中存储为 32 字节值。然而，Solidity 中的地址是 20 字节长，并在内存中存储为 32 字节值，其中前 12 字节被 0 替换。
> 因此，当我们需要跳过 12 字节以指向实际地址时。

5. `let start := add(ptr, 0x0b)` 创建一个新变量 `start`，指向内存位置 `ptr + 0x0b`，即 `ptr + 11 字节`。
6. 最后，mstore8 操作码可用于在内存位置存储单个字节。在这里，我们在 `start` 指向的内存位置存储值 `0xff`，该位置占用内存槽的第 12 字节。
7. 将所有值打包到正确的内存位置后，我们现在可以对从 `start` 开始的内存槽调用 `keccak256`，并将内存槽的长度作为第二个参数传递。这将返回一个 32 字节的哈希，我们可以截断以获得最终地址。

> ℹ️ **注意**
>
> 你可以查看这个工厂实现的完整代码 [这里](https://github.com/Genesis3800/CREATE2Factory/blob/main/src/Create2.sol)。
> 还可以查看 OpenZeppelin 的 [CREATE2 库实现](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/793d92a3331538d126033cbacb1ee5b8a7d95adc/contracts/utils/Create2.sol)，该实现为本教程提供了灵感。
> 最后，Forge 提供了一些开箱即用的 `CREATE2` 地址计算辅助函数。[查看它们](https://github.com/foundry-rs/forge-std/blob/f73c73d2018eb6a111f35e4dae7b4f27401e9421/src/StdUtils.sol#L122-L134)。

### 测试我们的工厂

在 `test` 目录中创建一个名为 `Create2.t.sol` 的文件。
初始化一个名为 `Create2Test` 的合约，如下所示：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import {Counter} from "../src/Counter.sol";
import {Create2} from "../src/Create2.sol";

contract Create2Test is Test {

}
```

初始化以下状态变量和 `setUp()` 函数：

```solidity
    Create2 internal create2;
    Counter internal counter;

    function setUp() public {
        create2 = new Create2();
        counter = new Counter();
    }
```

创建一个名为 `testDeterministicDeploy()` 的新函数，该函数：

1. 部署一个新的 `Create2` 合约实例。
2. 向我们将用于模拟所有后续调用者的特定地址分配 100 ETH，使用 `prank` 作弊码。
3. 设置 `salt` 和 `bytecode` 参数
4. 使用之前部署的 `Create2` 合约在确定性地址上部署 `Counter` 合约。
5. 通过断言计算的地址等于部署的地址，检查合约是否部署在正确的地址上。

```solidity
    function testDeterministicDeploy() public {
        vm.deal(address(0x1), 100 ether);

        vm.startPrank(address(0x1));  
        bytes32 salt = "12345";
        bytes memory creationCode = abi.encodePacked(type(Counter).creationCode);

        address computedAddress = create2.computeAddress(salt, keccak256(creationCode));
        address deployedAddress = create2.deploy(salt , creationCode);
        vm.stopPrank();

        assertEq(computedAddress, deployedAddress);  
    }
```

保存所有文件，并使用 `forge test --match-path test/Create2.t.sol -vvvv` 运行测试。
你的测试应该在没有错误的情况下通过。
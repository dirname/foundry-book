# 不变性测试

## 概述

不变性测试允许对一组不变性表达式进行测试，这些表达式针对预定义合约中的预定义函数调用的随机序列进行测试。在每次函数调用之后，都会断言所有定义的不变性。

不变性测试是一种强大的工具，可以揭示协议中的逻辑错误。由于函数调用序列是随机的，并且输入是模糊的，不变性测试可以揭示边缘情况和高度复杂协议状态下的错误假设和不正确逻辑。

不变性测试活动有两个维度，`runs` 和 `depth`：
- `runs`：生成和运行的函数调用序列的次数。
- `depth`：在给定的 `run` 中进行的函数调用次数。每次函数调用后都会断言所有定义的不变性。如果函数调用失败，`depth` 计数器仍然会增加。

这些和其他不变性配置方面在[这里](#configuring-invariant-test-execution)解释。

与在 Foundry 中通过在函数名称前加上 `test` 来运行标准测试类似，不变性测试通过在函数名称前加上 `invariant` 来表示（例如，`function invariant_A()`）。

`afterInvariant()` 函数在每次不变性运行结束时调用（如果声明了），允许进行后活动处理。此函数可用于记录活动指标（例如，选择器被调用的次数）和后模糊活动测试（例如，关闭所有头寸并断言所有资金能够退出系统）。

### 配置不变性测试执行

不变性测试执行由用户可以通过 Forge 配置原语控制的参数管理。配置可以全局应用或按测试应用。有关此主题的详细信息，请参阅
 📚 [`全局配置`](../reference/config/testing.md) 和 📚 [`内联配置`](../reference/config/inline-test-config.md)。

## 定义不变性

不变性是应该在整个模糊活动过程中始终保持为真的条件表达式。一个好的不变性测试套件应该尽可能多地包含不变性，并且可以为不同的协议状态提供不同的测试套件。

不变性的例子包括：
- *"xy=k 公式总是成立"* 对于 Uniswap
- *"所有用户余额的总和等于总供应量"* 对于 ERC-20 代币。

有不同的方式来断言不变性，如下表所述：

<table>
<tr><th>类型</th><th>解释</th><th>示例</th></tr>

<tr>

<td>直接断言</td>
<td>查询协议智能合约并断言值符合预期。</td>
<td>

```solidity
assertGe(
    token.totalAssets(),
    token.totalSupply()
)
```
</td>

</tr>

<tr>

<td>幽灵变量断言</td>
<td>查询协议智能合约并将其与测试环境中持久化的值（幽灵变量）进行比较。</td>
<td>

```solidity
assertEq(
    token.totalSupply(),
    sumBalanceOf
)
```
</td>

</tr>

<tr>

<td>去优化（朴素实现断言）</td>
<td>查询协议智能合约并将其与相同逻辑的朴素且通常非常低效的实现进行比较。</td>
<td>

```solidity
assertEq(
    pool.outstandingInterest(),
    test.naiveInterest()
)
```
</td>

</tr>
</table>

### 条件不变性

不变性必须在给定的模糊活动过程中保持，但这并不意味着它们必须在每种情况下都成立。在某些情况下，可能会有某些不变性被引入/移除（例如，在清算期间）。

不建议在不变性断言中引入条件逻辑，因为它们有可能引入误报，因为代码路径不正确。例如：

```solidity
function invariant_example() external {
    if (protocolCondition) return;

    assertEq(val1, val2);
}
```

在这种情况下，如果 `protocolCondition == true`，则根本不会断言不变性。有时这可能是期望的行为，但如果 `protocolCondition` 在整个模糊活动中意外为真，或者条件本身存在逻辑错误，则可能会导致问题。因此，最好尝试为该条件定义一个替代不变性，例如：

```solidity
function invariant_example() external {
    if (protocolCondition) {
        assertLe(val1, val2);
        return;
    };

    assertEq(val1, val2);
}
```

处理不同协议状态下的不同不变性的另一种方法是利用针对不同场景的专用不变性测试合约。这些场景可以使用 `setUp` 函数进行引导，但更强大的方法是利用 *不变性目标* 来控制模糊器以某种方式行为，从而仅产生某些结果（例如，避免清算）。

## 不变性目标

**目标合约**：在给定的不变性测试模糊活动中将被调用的一组合约。这组合约默认是 `setUp` 函数中部署的所有合约，但可以自定义以允许更高级的不变性测试。

**目标发送者**：不变性测试模糊器在执行模糊活动时随机选择 `msg.sender` 的值，以默认模拟系统中的多个参与者。如果需要，可以在 `setUp` 函数中自定义发送者集合。

**目标接口**：在 `setUp` 期间未部署但在分叉环境中模糊的地址及其项目标识符集合（例如，`[(0x1, ["IERC20"]), (0x2, ("IOwnable"))]`）。这使得可以针对代理和使用 `create` 或 `create2` 部署的合约进行模糊。

**目标选择器**：模糊器用于不变性测试的函数选择器集合。这些可以用于在给定目标合约中使用函数子集。

**目标构件**：给定合约使用的所需 ABI。这些可以用于代理合约配置。

**目标构件选择器**：在给定 ABI 中使用的所需函数选择器子集。这些可以用于代理合约配置。

在目标冲突的情况下，不变性模糊器的优先级是：

`targetInterfaces | targetSelectors > excludeSelectors | targetArtifactSelectors > excludeContracts | excludeArtifacts > targetContracts | targetArtifacts`

### 函数调用概率分布

这些合约中的函数将以随机（均匀分布的概率）调用，并带有模糊输入。

例如：

```text
targetContract1:
├─ function1: 20%
└─ function2: 20%

targetContract2:
├─ function1: 20%
├─ function2: 20%
└─ function3: 20%
```

在设计目标合约时需要注意这一点，因为函数较少的合约中的每个函数会由于这种概率分布而被更频繁地调用。

### 不变性测试辅助函数
不变性测试辅助函数包含在 [`forge-std`](https://github.com/foundry-rs/forge-std/blob/master/src/StdInvariant.sol) 中，以允许可配置的不变性测试设置。辅助函数如下所述：

| 函数 | 描述 |
|-|-|
| `excludeContract(address newExcludedContract_)` | 将给定地址添加到 `_excludedContracts` 数组中。这组合约明确排除在目标合约之外。|
| `excludeSelector(FuzzSelector memory newExcludedSelector_)` | 将给定 `FuzzSelector` 添加到 `_excludedSelectors` 数组中。这组 `FuzzSelector` 明确排除在目标合约选择器之外。 |
| `excludeSender(address newExcludedSender_)` | 将给定地址添加到 `_excludedSenders` 数组中。这组地址明确排除在目标发送者之外。 |
| `excludeArtifact(string memory newExcludedArtifact_)` | 将给定字符串添加到 `_excludedArtifacts` 数组中。这组字符串明确排除在目标构件之外。 |
| `targetArtifact(string memory newTargetedArtifact_)` | 将给定字符串添加到 `_targetedArtifacts` 数组中。这组字符串用于目标构件。  |
| `targetArtifactSelector(FuzzArtifactSelector memory newTargetedArtifactSelector_)` | 将给定 `FuzzArtifactSelector` 添加到 `_targetedArtifactSelectors` 数组中。这组 `FuzzArtifactSelector` 用于目标构件选择器。 |
| `targetContract(address newTargetedContract_)` | 将给定地址添加到 `_targetedContracts` 数组中。这组地址用于目标合约。这数组覆盖了 `setUp` 期间部署的合约集合。 |
| `targetSelector(FuzzSelector memory newTargetedSelector_)` | 将给定 `FuzzSelector` 添加到 `_targetedSelectors` 数组中。这组 `FuzzSelector` 用于目标合约选择器。 |
| `targetSender(address newTargetedSender_)` | 将给定地址添加到 `_targetedSenders` 数组中。这组地址用于目标发送者。 |
| `targetInterface(FuzzInterface memory newTargetedInterface_)` | 将给定 `FuzzInterface` 添加到 `_targetedInterfaces` 数组中。这组 `FuzzInterface` 扩展了要模糊的合约和选择器，并启用了在 `setUp` 期间未部署的地址的模糊，例如在分叉环境中模糊时。还启用了代理和使用 `create` 或 `create2` 部署的合约的模糊。 |

### 目标合约设置

目标合约可以通过以下三种方法设置：
1. 手动添加到 `targetContracts` 数组的合约将添加到目标合约集合中。
2. 在 `setUp` 函数中部署的合约将自动添加到目标合约集合中（仅在未使用选项 1 手动添加合约时有效）。
3. 在 `setUp` 中部署的合约可以**移除**目标合约，如果它们被添加到 `excludeContracts` 数组中。


## 开放测试

目标合约的默认配置设置为在设置期间部署的所有合约。对于较小的模块和更多算术合约，这效果很好。例如：

```solidity
contract ExampleContract1 {

    uint256 public val1;
    uint256 public val2;
    uint256 public val3;

    function addToA(uint256 amount) external {
        val1 += amount;
        val3 += amount;
    }

    function addToB(uint256 amount) external {
        val2 += amount;
        val3 += amount;
    }

}
```

这个合约可以部署并使用默认目标合约模式进行测试：

```solidity
contract InvariantExample1 is Test {

    ExampleContract1 foo;

    function setUp() external {
        foo = new ExampleContract1();
    }

    function invariant_A() external {
        assertEq(foo.val1() + foo.val2(), foo.val3());
    }

    function invariant_B() external {
        assertGe(foo.val1() + foo.val2(), foo.val3());
    }

}
```

这种设置将以 50%-50% 的概率分布调用 `foo.addToA()` 和 `foo.addToB()`，并带有模糊输入。不可避免地，输入将开始导致溢出，函数调用将开始失败。由于不变性测试中的默认配置是 `fail_on_revert = false`，这不会导致测试失败。不变性将在其余的模糊活动中保持，结果是测试将通过。输出将如下所示：

```text
[PASS] invariant_A() (runs: 50, calls: 10000, reverts: 5533)
[PASS] invariant_B() (runs: 50, calls: 10000, reverts: 5533)
```

## 基于处理器的测试

对于更复杂和集成的协议，需要更复杂的目标合约使用才能达到预期结果。为了说明如何利用处理器，以下合约将被使用（一个基于 ERC-4626 的合约，接受另一个 ERC-20 代币的存款）：

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.17;

interface IERC20Like {

    function balanceOf(address owner_) external view returns (uint256 balance_);

    function transferFrom(
        address owner_,
        address recipient_,
        uint256 amount_
    ) external returns (bool success_);

}

contract Basic4626Deposit {

    /**********************************************************************************************/
    /*** 存储                                                                                ***/
    /**********************************************************************************************/

    address public immutable asset;

    string public name;
    string public symbol;

    uint8 public immutable decimals;

    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;

    /**********************************************************************************************/
    /*** 构造函数                                                                            ***/
    /**********************************************************************************************/

    constructor(address asset_, string memory name_, string memory symbol_, uint8 decimals_) {
        asset    = asset_;
        name     = name_;
        symbol   = symbol_;
        decimals = decimals_;
    }

    /**********************************************************************************************/
    /*** 外部函数                                                                     ***/
    /**********************************************************************************************/

    function deposit(uint256 assets_, address receiver_) external returns (uint256 shares_) {
        shares_ = convertToShares(assets_);

        require(receiver_ != address(0), "ZERO_RECEIVER");
        require(shares_   != uint256(0), "ZERO_SHARES");
        require(assets_   != uint256(0), "ZERO_ASSETS");

        totalSupply += shares_;

        // Cannot overflow because totalSupply would first overflow in the statement above.
        unchecked { balanceOf[receiver_] += shares_; }

        require(
            IERC20Like(asset).transferFrom(msg.sender, address(this), assets_),
            "TRANSFER_FROM"
        );
    }

    function transfer(address recipient_, uint256 amount_) external returns (bool success_) {
        balanceOf[msg.sender] -= amount_;

        // Cannot overflow because minting prevents overflow of totalSupply,
        // and sum of user balances == totalSupply.
        unchecked { balanceOf[recipient_] += amount_; }

        return true;
    }

    /**********************************************************************************************/
    /*** 公共视图函数                                                                  ***/
    /**********************************************************************************************/

    function convertToShares(uint256 assets_) public view returns (uint256 shares_) {
        uint256 supply_ = totalSupply;  // Cache to stack.

        shares_ = supply_ == 0 ? assets_ : (assets_ * supply_) / totalAssets();
    }

    function totalAssets() public view returns (uint256 assets_) {
        assets_ = IERC20Like(asset).balanceOf(address(this));
    }

}

```

### 处理器函数

这个合约的 `deposit` 函数要求调用者拥有 ERC-20 `asset` 的非零余额。在开放不变性测试方法中，`deposit()` 和 `transfer()` 将以 50-50% 的分布调用，但它们会在每次调用时失败。这将导致不变性测试“通过”，但实际上根本没有在所需合约中操纵任何状态。这就是可以利用目标合约的地方。当合约需要一些额外的逻辑才能正常工作时，可以在一个称为 `Handler` 的专用合约中添加它。

```solidity
function deposit(uint256 assets) public virtual {
    asset.mint(address(this), assets);

    asset.approve(address(token), assets);

    uint256 shares = token.deposit(assets, address(this));
}
```

这个合约将在进行函数调用之前提供必要的设置，以确保其成功。

在此概念的基础上，处理器可以用于开发更复杂的不变性测试。通过开放不变性测试，测试运行如下图所示，随机序列的函数调用直接使用模糊参数对协议合约进行调用。这将导致更复杂系统中的失败，如上所述。

![Blank diagram](https://user-images.githubusercontent.com/44272939/214752968-5f0e7653-d52e-43e6-b453-cac935f5d97d.svg)

通过手动将所有处理器合约添加到 `targetContracts` 数组中，可以确保对协议合约的所有函数调用都由处理器控制，以确保成功调用。这在下图中有详细说明。

![Invariant Diagrams - Page 2](https://user-images.githubusercontent.com/44272939/216420091-8a5c2bcc-d586-458f-be1e-a9ea0ef5961f.svg)

通过这种模糊器和协议之间的层，可以实现更强大的测试。

### 处理器幽灵变量

在处理器中，可以跨多个函数调用跟踪“幽灵变量”，以为不变性测试添加额外信息。一个很好的例子是将在 ERC-4626 代币中存款后每个 LP 拥有的 `shares` 总和，并使用该不变性（`totalSupply == sumBalanceOf`）。

```solidity
function deposit(uint256 assets) public virtual {
    asset.mint(address(this), assets);

    asset.approve(address(token), assets);

    uint256 shares = token.deposit(assets, address(this));

    sumBalanceOf += shares;
}
```

### 函数级断言

另一个好处是能够在函数调用发生时进行断言。一个例子是在 `deposit` 函数调用期间断言 LP 的 ERC-20 余额减少了 `assets`，以及他们的 LP 代币余额增加了 `shares`。通过这种方式，处理器函数类似于模糊测试，因为它们可以接受模糊输入，执行状态更改，并在前后状态中断言。

```solidity
function deposit(uint256 assets) public virtual {
    asset.mint(address(this), assets);

    asset.approve(address(token), assets);

    uint256 beforeBalance = asset.balanceOf(address(this));

    uint256 shares = token.deposit(assets, address(this));

    assertEq(asset.balanceOf(address(this)), beforeBalance - assets);

    sumBalanceOf += shares;
}
```

### 有界/无界函数

此外，通过处理器，输入参数可以被限制为合理的预期值，使得 `foundry.toml` 中的 `fail_on_revert` 可以设置为 `true`。这可以通过 `forge-std` 中的 `bound()` 辅助函数来实现。这确保了模糊器进行的每个函数调用都必须对协议成功，才能通过测试。这对于可见性和确保协议以期望的方式进行测试的信心非常有用。

```solidity
function deposit(uint256 assets) external {
    assets = bound(assets, 0, 1e30);

    asset.mint(address(this), assets);

    asset.approve(address(token), assets);

    uint256 beforeBalance = asset.balanceOf(address(this));

    uint256 shares = token.deposit(assets, address(this));

    assertEq(asset.balanceOf(address(this)), beforeBalance - assets);

    sumBalanceOf += shares;
}
```

这也可以通过从专用“无界”处理器合约继承非有界函数来实现，这些合约可用于 `fail_on_revert = false` 测试。这种测试也很有用，因为它可以揭示 `bound` 函数使用中假设的问题。

```solidity
// 无界
function deposit(uint256 assets) public virtual {
    asset.mint(address(this), assets);

    asset.approve(address(token), assets);

    uint256 beforeBalance = asset.balanceOf(address(this));

    uint256 shares = token.deposit(assets, address(this));

    assertEq(asset.balanceOf(address(this)), beforeBalance - assets);

    sumBalanceOf += shares;
}
```

```solidity
// 有界
function deposit(uint256 assets) external {
    assets = bound(assets, 0, 1e30);

    super.deposit(assets);
}
```

### 参与者管理

在上面的函数调用中，可以看到 `address(this)` 是 ERC-4626 合约中的唯一存款者，这并不是其预期用途的真实表示。通过利用 `forge-std` 中的 `prank` 作弊代码，每个处理器可以管理一组参与者，并使用它们从不同的 `msg.sender` 地址执行相同的函数调用。这可以通过以下修饰符实现：

```solidity
address[] public actors;

address internal currentActor;

modifier useActor(uint256 actorIndexSeed) {
    currentActor = actors[bound(actorIndexSeed, 0, actors.length - 1)];
    vm.startPrank(currentActor);
    _;
    vm.stopPrank();
}
```

使用多个参与者还允许更细粒度的幽灵变量使用。这在下面的函数中有所展示：

```solidity
// 无界
function deposit(
    uint256 assets,
    uint256 actorIndexSeed
) public virtual useActor(actorIndexSeed) {
    asset.mint(currentActor, assets);

    asset.approve(address(token), assets);

    uint256 beforeBalance = asset.balanceOf(address(this));

    uint256 shares = token.deposit(assets, address(this));

    assertEq(asset.balanceOf(address(this)), beforeBalance - assets);

    sumBalanceOf += shares;

    sumDeposits[currentActor] += assets
}
```

```solidity
// 有界
function deposit(uint256 assets, uint256 actorIndexSeed) external {
    assets = bound(assets, 0, 1e30);

    super.deposit(assets, actorIndexSeed);
}
```
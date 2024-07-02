## 不变测试债券曲线

### 介绍

本教程将介绍不变测试，使用**债券曲线实现**作为目标示例。所有不变测试都是使用 `Foundry Invaraint Testing` 功能在 Solidity 中编写的。

然而，本指南仅用于教育目的。代码未经过审计，请勿在生产环境中使用。

> 💡 注意：完整的债券曲线实现可以在[这里](https://github.com/Ratimon/bonding-curves)找到，关于不变测试的进一步阅读，可以查看 `Invariant Testing` [参考](../reference/forge/invariant-testing.md)。

### 快速开始

不变测试的一般过程是 Foundry 会在目标合约中使用随机输入调用一系列预定义的**动作**。

这将多次运行以确保不变性的正确性。

> 💡 注意：使用随机输入进行动作调用的过程称为**模糊测试**。

> 💡 注意：生成并运行一系列函数调用的次数称为**运行**。

每次单次运行后，系统将被检查以查看定义的**不变性**是否保持为真。

关键考虑是定义这些：

1. **不变性**（一组始终为真的属性）

2. **动作**（每次运行期间可能发生的一组事情）

要开始，我们将关注以下[仓库](https://github.com/Ratimon/bonding-curves)中的目录：

```
.
├── Makefile
├── foundry.toml
└── test
    ├── invariant
```

在本指南中，我们可以通过运行以下命令来运行模糊测试活动：

```sh
make invariant-LinearBondingCurve
```

> 💡 注意：本教程的其他命令可以在 [ `Makefile`](https://github.com/Ratimon/bonding-curves/blob/master/Makefile) 中找到。

我注意到不变测试的默认配置（在 [ `foundry.toml`](https://github.com/Ratimon/bonding-curves/blob/master/foundry.toml)）如下：

```toml
[invariant]
runs          = 1000    # 运行不变测试的次数
depth         = 100   # 不变测试中随机动作调用的次数
fail_on_revert = false
```

### 定义不变性

要指定不变性，我们需要构建关于可能达到的错误状态的思维模型。

> 💡 注意：不变性是关于某事物（在我们的例子中是合约）的逻辑断言的术语，始终为真。

我们可以定义两种不变性，包括：

1. 函数级不变性

- 它们应该是无状态的，或者不依赖于我们的目标系统。
- 它们可以以隔离的方式进行测试。

> 💡 注意：显著的例子可能是将代币存入合约。

2. 系统级不变性

- 它们应该是有状态的，或者依赖于智能合约状态。
- 它们依赖于整个系统及其相关的部署逻辑。

> 💡 注意：例如，ERC20 的系统级不变性是 ERC20 代币的总数始终等于或大于所有代币持有者的 ERC20 代币余额之和。

### 定义系统级不变性

在我们的例子中，用于定义不变性的状态变量（在 [`BondingCurve.sol`](https://github.com/Ratimon/bonding-curves/blob/master/src/bondingcurves/BondingCurve.sol) 中找到）如下：

```solidity
    /** ... */
    abstract contract BondingCurve is IBondingCurve, Initializable, Pausable, Ownable2Step, Timed {
        /** ... */
        /**
        * @notice 在债券曲线上购买的销售代币总量
        *
        */
        UD60x18 public override totalPurchased;

        /**
        * @notice 债券曲线可以铸造的销售代币上限
        *
        */
        UD60x18 public override cap;

        /**
        * @notice 返回我们离上限有多近
        *
        */
        function availableToSell() public view override returns (UD60x18) {
            return cap.sub(totalPurchased);
        }

        /**
        * @notice 债券曲线的接受代币余额
        * @return 合约中持有的接受代币数量，准备分配
        *
        */
        function reserveBalance() public view virtual override returns (UD60x18) {
            return ud(acceptedToken.balanceOf(address(this)));
        }

    /** ... */

    }

```

然后，我们可以在 [`LinearBondingCurve.invariants.t.sol`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/LinearBondingCurve.invariants.t.sol) 中指定并编写断言，如下：

1. 不变性 1：totalPurchased + availableToSell = cap

```solidity

    function invariant_totalPurchasedPlusAvailableToSell_eq_cap() public {
        assertEq(
            unwrap(linearBondingCurve.totalPurchased().add(linearBondingCurve.availableToSell())),
            unwrap(linearBondingCurve.cap())
        );
    }

```

2. 不变性 2：availableToSell >= 0

```solidity

    function invariant_AvailableToSell_gt_zero() public {
        assertGt(unwrap(linearBondingCurve.availableToSell()), 0);
    }


```

3. 不变性 3：availableToSell = 债券曲线合约中的 ERC20 代币数量

```solidity
    function invariant_AvailableToSell_eq_saleTokenBalance() public {
        assertEq(unwrap(linearBondingCurve.availableToSell()), IERC20(saleToken).balanceOf(address(linearBondingCurve)));
    }
```

4. 不变性 4：Poolbalance = y = f(x = currentTokenPurchased) = slope/2 * (currentTokenPurchased)^2 + initialPrice * (currentTokenPurchased)

```solidity
    function invariant_Poolbalance_eq_saleTokenBalance() public {
        UD60x18 acceptedTokenSupply = ud(IERC20(acceptedToken).balanceOf(address(linearBondingCurve)));
        assertEq(
            unwrap(LinearCurve(address(linearBondingCurve)).getPoolBalance(acceptedTokenSupply)),
            unwrap(linearBondingCurve.totalPurchased())
        );
    }
```

### 定义动作逻辑和函数级不变性

好的！我们已经定义了一些系统级不变性。下一步是指定如何执行动作和相关交易的序列以破坏定义的不变性。

高级内容在 [`LinearBondingCurve.invariants.t.sol`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/LinearBondingCurve.invariants.t.sol) 中探索，配置在 `setup()` 中如下：

```solidity

import {InvariantOwner} from "./handlers/Owner.sol";
import {InvariantBuyerManager} from "./handlers/Buyer.sol";
import {Warper} from "./handlers/Warper.sol";

contract LinearBondingCurveInvariants is StdInvariant, Test, ConstantsFixture, DeploymentLinearBondingCurve {
    InvariantOwner internal _owner;
    InvariantBuyerManager internal _buyerManager;
    Warper internal _warper;

    /** ... */

        function setUp() public override {
            /** ... */
            _buyerManager =
                new InvariantBuyerManager(address(linearBondingCurve), address(acceptedToken),  address(saleToken) );
            _warper = new Warper(address(linearBondingCurve));
            _owner = new InvariantOwner(address(linearBondingCurve), address(acceptedToken), address(saleToken), staticTime);

            /** ... */

            vm.startPrank(address(_owner));
            Ownable2Step(address(linearBondingCurve)).acceptOwnership();
            vm.stopPrank();

            vm.startPrank(deployer);
            bytes4[] memory selectors = new bytes4[](1);
            selectors[0] = InvariantBuyerManager.purchase.selector;

            // 执行随机的 purchase() 调用
            targetSelector(FuzzSelector({addr: address(_buyerManager), selectors: selectors}));
            targetContract(address(_buyerManager));

            selectors[0] = Warper.warp.selector;
            // 执行随机的时间推进
            targetSelector(FuzzSelector({addr: address(_warper), selectors: selectors}));
            targetContract(address(_warper));

            selectors[0] = InvariantOwner.allocate.selector;
            // 执行随机的 allocate() 调用
            targetSelector(FuzzSelector({addr: address(_owner), selectors: selectors}));
            targetContract(address(_owner));

            _buyerManager.createBuyer();
            vm.stopPrank();
        }
    /** ... */
    }

```

总结一下，我们执行随机的 `purchase()` 调用，随机的时间推进，以及随机的 `allocate()` 调用。

这是通过**不变测试辅助函数**（包括 **targetContract(address newTargetedContract_)** 和 **targetSelector(FuzzSelector memory newTargetedSelector_)**）设置的。

> 💡 注意：更多细节在 `foundry documentation` 的 [`Invariant Test Helper Functions`](https://book.getfoundry.sh/forge/invariant-testing#invariant-test-helper-functions) 部分中概述

我们可以将**Foundry Fuzzer**视为外部拥有的账户，将**Handler**视为智能合约包装器，包括一组与我们的目标合约交互的动作。

这些处理程序在 [`test/invariant/handlers`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/handlers) 中的**处理程序文件**中指定，如下：

1. [`Buyer.sol`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/handlers/Buyer.sol)：执行随机的 `purchase()`

我们可以将这组智能合约视为具有外部利益相关者属性。通常，我们希望 fuzzer 生成多个买家。现在，我们定义 **InvariantBuyerManager** 如下。

可以看到，**createBuyer()** 在我们的 `setup()` 中被调用，因此我们有一个买家（**InvariantBuyer**）从 **InvariantBuyerManager** 生成。

```solidity

contract InvariantBuyerManager is Test {

    /** ... */

    InvariantBuyer[] public buyers;

    /** ... */

    function createBuyer() external {
        InvariantBuyer buyer = new InvariantBuyer(_bondingCurve, _underlyingAcceptedToken, _underlyingSaleToken);
        buyers.push(buyer);
    }

    /** ... */

}
```

然后，那些生成的买家（**InvariantBuyer**）应该从债券曲线购买代币。现在，我们将编写**函数级不变性**。

特别是，我们要确保外部智能合约（在我们的例子中是 ERC20 代币）的余额状态在每次购买后都正确更新。实现如下：

```solidity

/** ... */

contract InvariantBuyer is Test {
    LinearBondingCurve internal _bondingCurve;
    MockERC20 internal _underlyingAcceptedToken;
    MockERC20 internal _underlyingSaleToken;

    constructor(address bondingCurve_, address underlyingBuyToken_, address underlyingSaleToken_) {
        _bondingCurve = LinearBondingCurve(bondingCurve_);
        _underlyingAcceptedToken = MockERC20(underlyingBuyToken_);
        _underlyingSaleToken = MockERC20(underlyingSaleToken_);
    }

    function purchase(uint256 amount_) external {
        amount_ = bound(amount_, 1, 1e29); // 1000 亿以 WAD 精度
        uint256 startingBuyBalance = _underlyingAcceptedToken.balanceOf(address(this));
        uint256 startingSaleBalance = _underlyingSaleToken.balanceOf(address(this));
        uint256 saleAmountOut = unwrap(_bondingCurve.calculatePurchaseAmountOut(ud(amount_)));
        deal({token: address(_underlyingAcceptedToken), to: address(this), give: amount_});
        _underlyingAcceptedToken.approve(address(_bondingCurve), amount_);
        _bondingCurve.purchase(address(this), amount_);
        // 确保购买成功
        assertEq(_underlyingAcceptedToken.balanceOf(address(this)), startingBuyBalance - amount_);
        assertEq(_underlyingSaleToken.balanceOf(address(this)), startingSaleBalance + saleAmountOut);
    }
}

/** ... */

```

2. [`Owner.sol`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/handlers/Owner.sol)：执行随机的 `allocate()`

同样，我们确保外部合约（可接受代币）的余额状态在代币分配给发行人后正确更新。实现如下：

```solidity

/** ... */

contract InvariantOwner is Test {

    /** ... */

    function allocate(uint256 amount_) external countCall("allocate") {
        vm.warp(staticTime + 3 weeks);
        amount_ = bound(amount_, 0, _underlyingAcceptedToken.balanceOf(address(_bondingCurve)));
        uint256 startingBuyBalance = _underlyingAcceptedToken.balanceOf(address(this));
        _bondingCurve.allocate(amount_, address(this));
        assertEq(_underlyingAcceptedToken.balanceOf(address(this)), startingBuyBalance + amount_);
    }

    /** ... */

}

/** ... */

```

3. [`Warper.sol`](https://github.com/Ratimon/bonding-curves/blob/master/test/invariant/handlers/Warper.sol)：执行随机的时间推进。

由于该系统涉及时间依赖逻辑，发行人在销售期结束前不能分配代币。这意味着没有 `warps` 处理程序的随机 `allocate()` 总是会回滚。

为此，我们使用 `Foundry's cheat code`（**vm.warp(uint256)**）来处理它。

```solidity

/** ... */

contract Warper is CommonBase, StdCheats, StdUtils {

    /** ... */

    function warp(uint256 warpTime_) external countCall("warp") {
        vm.warp(block.timestamp + bound(warpTime_, 2 weeks, 3 weeks));
    }

    /** ... */

}

/** ... */

```

### 探索更多！

> 💡 注意：我们承认、使用并从项目 [PaulRBerg/prb-math](https://github.com/PaulRBerg/prb-math) 和 [maple-labs/revenue-distribution-token](https://github.com/maple-labs/revenue-distribution-token) 中获得灵感。

> 💡 参考：我们也承认有见地的技术写作：[Invariant Testing WETH With Foundry](https://mirror.xyz/horsefacts.eth/Jex2YVaO65dda6zEyfM_-DXlXhOWCAoSpOx5PLocYgw) 和 [Invariant Testing — Enter The Matrix](https://betterprogramming.pub/invariant-testing-enter-the-matrix-c71363dea37e)
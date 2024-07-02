## Vyper 支持

Foundry 支持编译和测试 Vyper 合约。

### 1. 编译

您可以按照[这里](https://vyper.readthedocs.io/en/stable/installing-vyper.html)的说明安装 Vyper。如果您的 PATH 中有 `vyper`，Foundry 将自动使用它。

否则，您可以在 `foundry.toml` 中通过添加以下内容来设置 `vyper` 的路径：
```toml
[vyper]
path = "/path/to/vyper"
```

### 2. Solidity 测试

让我们为这个简单的 Counter 合约编写一个测试：

```vyper
number: public(uint256)

@deploy
@payable
def __init__(initial_number: uint256):
    self.number = initial_number

@external
def set_number(new_number: uint256):
    self.number = new_number

@external
def increment():
    self.number += 1
```

我们可以使用 `forge-std` 中的 `deployCode` 作弊码来部署它，并用以下 Solidity 测试来测试它：
```solidity
import {Test} from "forge-std/Test.sol";

interface ICounter {
    function increment() external;
    function number() external view returns (uint256);
    function set_number(uint256 newNumber) external;
}

contract CounterTest is Test {
    ICounter public counter;
    uint256 initialNumber = 5;

    function setUp() public {
        counter = ICounter(deployCode("Counter", abi.encode(initialNumber)));
        assertEq(counter.number(), initialNumber);
    }

    function test_Increment() public {
        counter.increment();
        assertEq(counter.number(), initialNumber + 1);
    }

    function testFuzz_SetNumber(uint256 x) public {
        counter.set_number(x);
        assertEq(counter.number(), x);
    }
}
```

### 3. 部署

您可以通过 `forge create` 命令部署 Vyper 合约：
```bash
forge create Counter --constructor-args '1' --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

通过 `deployCode`，您也可以在脚本中部署 Vyper 合约：
```solidity
import {Script} from "forge-std/Script.sol";

contract CounterScript is Script {
    function run() public {
        vm.broadcast();
        deployCode("src/Counter.vy", abi.encode(1));
    }
}
```

### 4. Vyper 脚本

您可以像编写 Solidity 脚本一样编写 Vyper 脚本：
```vyper
interface Vm:
    def startBroadcast(): nonpayable

interface ICounter:
    def increment(): nonpayable
    def number() -> uint256: view

vm: constant(Vm) = Vm(0x7109709ECfa91a80626fF3989D68f67F5b1DD12D)

@external
def run(counter: address):
    number_before: uint256 = staticcall ICounter(counter).number()

    extcall vm.startBroadcast()
    extcall ICounter(counter).increment()

    number_after: uint256 = staticcall ICounter(counter).number()

    assert number_after == number_before + 1
```

这样的脚本可以通过以下命令运行：
```bash
forge script script/Increment.s.vy  --sig 'run' '<counter address>' --rpc-url $RPC_URL --broadcast  --private-key $PRIVATE_KEY
```

### 5. 限制

- 虽然您可以编写和运行 Vyper 测试和脚本，但 Vyper 中没有 `new` 关键字允许您部署合约。这将在未来通过新的作弊码得到解决。
- Vyper 不允许具有相同名称但参数类型不同的重载。因此，某些作弊码组合可能需要使用变通方法。（例如 `startBroadcast(address sender)` 和 `startBroadcast(uint256 pk)`）
- `forge coverage` 目前不支持 Vyper 合约。
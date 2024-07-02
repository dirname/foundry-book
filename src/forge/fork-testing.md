## 分叉测试

Forge 支持在分叉环境中进行测试，有两种不同的方法：

- [**分叉模式**](#forking-mode) — 通过 `forge test --fork-url` 标志使用单个分叉进行所有测试
- [**分叉作弊码**](#forking-cheatcodes) — 直接在 Solidity 测试代码中创建、选择和管理多个分叉，使用 [分叉作弊码](../cheatcodes/forking.md)

使用哪种方法？分叉模式允许针对特定的分叉环境运行整个测试套件，而分叉作弊码提供更多的灵活性和表达性，以便在测试中使用多个分叉。您的具体用例和测试策略将有助于决定使用哪种方法。

### 分叉模式

要在分叉环境中运行所有测试，例如分叉的以太坊主网，通过 `--fork-url` 标志传递一个 RPC URL：

```bash
forge test --fork-url <your_rpc_url>
```

以下值将更改为分叉时刻的链上值：

- [`block_number`](../reference/config/testing.md#block_number)
- [`chain_id`](../reference/config/testing.md#chain_id)
- [`gas_limit`](../reference/config/testing.md#gas_limit)
- [`gas_price`](../reference/config/testing.md#gas_price)
- [`block_base_fee_per_gas`](../reference/config/testing.md#block_base_fee_per_gas)
- [`block_coinbase`](../reference/config/testing.md#block_coinbase)
- [`block_timestamp`](../reference/config/testing.md#block_timestamp)
- [`block_difficulty`](../reference/config/testing.md#block_difficulty)

可以使用 `--fork-block-number` 指定从哪个区块分叉：

```bash
forge test --fork-url <your_rpc_url> --fork-block-number 1
```

分叉在需要与现有合约交互时特别有用。您可以选择这种方式进行集成测试，就像在实际网络上一样。

#### 缓存

如果同时指定了 `--fork-url` 和 `--fork-block-number`，则该区块的数据会缓存以供将来测试运行。

数据缓存路径为 `~/.foundry/cache/rpc/<chain name>/<block number>`。要清除缓存，只需删除目录或运行 [`forge clean`](../reference/forge/forge-clean.md)（删除所有构建工件和缓存目录）。

也可以通过传递 `--no-storage-caching` 完全忽略缓存，或在 `foundry.toml` 中通过配置 [`no_storage_caching`](../reference/config/testing.md#no_storage_caching) 和 [`rpc_storage_caching`](../reference/config/testing.md#rpc_storage_caching) 来实现。

#### 改进的跟踪

Forge 支持在分叉环境中通过 Etherscan 识别合约。

要使用此功能，通过 `--etherscan-api-key` 标志传递 Etherscan API 密钥：

```bash
forge test --fork-url <your_rpc_url> --etherscan-api-key <your_etherscan_api_key>
```

或者，您可以设置 `ETHERSCAN_API_KEY` 环境变量。

### 分叉作弊码

分叉作弊码允许您在 Solidity 测试代码中以编程方式进入分叉模式。与通过 `forge` CLI 参数配置分叉模式不同，这些作弊码允许您在测试中按需使用分叉模式，并在测试中使用多个分叉。每个分叉通过其唯一的 `uint256` 标识符进行识别。

#### 使用方法

重要的是要记住，_所有_ 测试函数都是隔离的，这意味着每个测试函数都在其自己的独立 EVM 中执行，状态是 `setUp` 之后的副本。

因此，在 `setUp` 期间创建的分叉在测试中可用。下面的代码示例使用 [`createFork`](../cheatcodes/create-fork.md) 创建两个分叉，但最初不选择其中一个。每个分叉通过唯一的标识符（`uint256 forkId`）进行识别，该标识符在首次创建时分配。

通过将该 `forkId` 传递给 [`selectFork`](../cheatcodes/select-fork.md) 来启用特定分叉。

[`createSelectFork`](../cheatcodes/create-select-fork.md) 是 `createFork` 加上 `selectFork` 的一行代码。

同一时间只能有一个分叉处于活动状态，当前活动分叉的标识符可以通过 [`activeFork`](../cheatcodes/active-fork.md) 获取。

类似于 [`roll`](../cheatcodes/roll.md)，您可以使用 [`rollFork`](../cheatcodes/roll-fork.md) 设置分叉的 `block.number`。

要理解选择分叉时会发生什么，重要的是要知道分叉模式的一般工作原理：

每个分叉都是一个独立的 EVM，即所有分叉使用完全独立的存储。唯一的例外是 `msg.sender` 和测试合约本身的状态，这些状态在分叉切换时是持久的。换句话说，在分叉 `A` 处于活动状态时（`selectFork(A)`）所做的所有更改仅记录在分叉 `A` 的存储中，如果选择另一个分叉，则不可用。然而，记录在测试合约本身（变量）中的更改仍然可用，因为测试合约是一个 _持久_ 账户。

`selectFork` 作弊码设置带有分叉数据源的 _远程_ 部分，然而 _本地_ 内存跨分叉切换保持持久。这也意味着 `selectFork` 可以在任何时候使用任何分叉调用，以设置 _远程_ 数据源。然而，重要的是要记住上述 `读/写` 访问规则始终适用，意味着 _写_ 操作跨分叉切换是持久的。

#### 示例

##### 创建和选择分叉

```solidity
contract ForkTest is Test {
    // 分叉的标识符
    uint256 mainnetFork;
    uint256 optimismFork;

    // 通过 vm.envString("varname") 访问 .env 文件中的变量
    // 将 ALCHEMY_KEY 替换为您的 alchemy 密钥或 Etherscan 密钥，根据需要更改 RPC url
    // 在您的 .env 文件中，例如：
    // MAINNET_RPC_URL = 'https://eth-mainnet.g.alchemy.com/v2/ALCHEMY_KEY'
    // string MAINNET_RPC_URL = vm.envString("MAINNET_RPC_URL");
    // string OPTIMISM_RPC_URL = vm.envString("OPTIMISM_RPC_URL");

    // 在 setup 期间创建两个 _不同的_ 分叉
    function setUp() public {
        mainnetFork = vm.createFork(MAINNET_RPC_URL);
        optimismFork = vm.createFork(OPTIMISM_RPC_URL);
    }

    // 演示分叉标识符是唯一的
    function testForkIdDiffer() public {
        assert(mainnetFork != optimismFork);
    }

    // 选择特定的分叉
    function testCanSelectFork() public {
        // 选择分叉
        vm.selectFork(mainnetFork);
        assertEq(vm.activeFork(), mainnetFork);

        // 从这里开始，如果 EVM 请求数据，则从 `mainnetFork` 获取数据并写入 `mainnetFork` 的存储
    }

    // 在同一个测试中管理多个分叉
    function testCanSwitchForks() public {
        vm.selectFork(mainnetFork);
        assertEq(vm.activeFork(), mainnetFork);

        vm.selectFork(optimismFork);
        assertEq(vm.activeFork(), optimismFork);
    }

    // 可以在任何时候创建分叉
    function testCanCreateAndSelectForkInOneStep() public {
        // 创建一个新的分叉并选择它
        uint256 anotherFork = vm.createSelectFork(MAINNET_RPC_URL);
        assertEq(vm.activeFork(), anotherFork);
    }

    // 设置分叉的 `block.number`
    function testCanSetForkBlockNumber() public {
        vm.selectFork(mainnetFork);
        vm.rollFork(1_337_000);

        assertEq(block.number, 1_337_000);
    }
}
```

##### 分离和持久的存储

如前所述，每个分叉本质上是一个独立的 EVM，具有分离的存储。

只有 `msg.sender` 和测试合约（`ForkTest`）的账户在分叉选择时是持久的。但任何账户都可以变成持久账户：[`makePersistent`](../cheatcodes/make-persistent.md)。

一个 _持久_ 账户是唯一的，即它存在于所有分叉上。

```solidity
contract ForkTest is Test {
    // 分叉的标识符
    uint256 mainnetFork;
    uint256 optimismFork;

    // 通过 vm.envString("varname") 访问 .env 文件中的变量
    // 将 ALCHEMY_KEY 替换为您的 alchemy 密钥或 Etherscan 密钥，根据需要更改 RPC url
    // 在您的 .env 文件中，例如：
    // MAINNET_RPC_URL = 'https://eth-mainnet.g.alchemy.com/v2/ALCHEMY_KEY'
    // string MAINNET_RPC_URL = vm.envString("MAINNET_RPC_URL");
    // string OPTIMISM_RPC_URL = vm.envString("OPTIMISM_RPC_URL");

    // 在 setup 期间创建两个 _不同的_ 分叉
    function setUp() public {
        mainnetFork = vm.createFork(MAINNET_RPC_URL);
        optimismFork = vm.createFork(OPTIMISM_RPC_URL);
    }

    // 在一个分叉处于活动状态时创建新合约
    function testCreateContract() public {
        vm.selectFork(mainnetFork);
        assertEq(vm.activeFork(), mainnetFork);

        // 新合约写入 `mainnetFork` 的存储
        SimpleStorageContract simple = new SimpleStorageContract();

        // 并可以正常使用
        simple.set(100);
        assertEq(simple.value(), 100);

        // 切换到另一个合约后，我们仍然知道 `address(simple)`，但合约仅存在于 `mainnetFork` 中
        vm.selectFork(optimismFork);

        /* 此调用将因此恢复，因为 `simple` 现在指向一个在活动分叉上不存在的合约
        * 它将产生以下恢复消息：
        *
        * "Contract 0xCe71065D4017F316EC606Fe4422e11eB2c47c246 does not exist on active fork with id `1`
        *       But exists on non active forks: `[0]`"
        */
        simple.value();
    }

     // 在一个分叉处于活动状态时创建新的 _持久_ 合约
     function testCreatePersistentContract() public {
        vm.selectFork(mainnetFork);
        SimpleStorageContract simple = new SimpleStorageContract();
        simple.set(100);
        assertEq(simple.value(), 100);

        // 将合约标记为持久，以便在其他分叉处于活动状态时也可用
        vm.makePersistent(address(simple));
        assert(vm.isPersistent(address(simple)));

        vm.selectFork(optimismFork);
        assert(vm.isPersistent(address(simple)));

        // 这将成功，因为合约现在在 `optimismFork` 上也可用
        assertEq(simple.value(), 100);
     }
}

contract SimpleStorageContract {
    uint256 public value;

    function set(uint256 _value) public {
        value = _value;
    }
}
```

有关更多详细信息和示例，请参阅 [分叉作弊码](../cheatcodes/forking.md) 参考。
## 作弊码参考

作弊码提供了强大的断言功能，能够改变 EVM 的状态，模拟数据等。

作弊码通过作弊码地址（`0x7109709ECfa91a80626fF3989D68f67F5b1DD12D`）提供。

> ℹ️ **注意**
>
> 如果在使用模糊测试地址时遇到此地址的错误，你可能希望通过以下行将其从模糊测试中排除：
>
> ```solidity
> vm.assume(address_ != 0x7109709ECfa91a80626fF3989D68f67F5b1DD12D);
> ```

你也可以通过 Forge 标准库的 [`Test`](../reference/forge-std/#forge-stds-test) 合约中可用的 `vm` 轻松访问作弊码。

### Forge 标准库作弊码

Forge Std 实现了作弊码的包装器，这些包装器结合了多个标准作弊码以改善开发体验。这些并不是技术上的作弊码，而是 Forge 作弊码的组合。

你可以在[参考部分](../reference/forge-std/std-cheats.md)查看 Forge 标准库的作弊码包装器列表。你可以参考 [Forge Std 源代码](https://github.com/foundry-rs/forge-std/blob/master/src/Test.sol) 了解更多关于包装器如何在底层工作的信息。

### 作弊码类型

以下是不同 Forge 作弊码的子部分。

- [环境](./environment.md)：改变 EVM 状态的作弊码。
- [断言](./assertions.md)：强大的断言作弊码。
- [模糊测试](./fuzzer.md)：配置模糊测试的作弊码。
- [外部](./external.md)：与外部状态（文件、命令等）交互的作弊码。
- [实用工具](./utilities.md)：较小的实用工具作弊码。
- [分叉](./forking.md)：分叉模式作弊码。
- [快照](./snapshots.md)：快照作弊码。
- [RPC](./rpc.md)：与 RPC 相关的作弊码。
- [文件](./fs.md)：处理文件的作弊码。

### 添加新的作弊码

如果你需要新功能，请考虑[为 Foundry 的代码库贡献](../contributing.md)以添加作弊码。

### 作弊码接口

这是 Forge 中所有作弊码的 Solidity 接口。

```solidity
interface CheatCodes {
    // 这允许我们 getRecordedLogs()
    struct Log {
        bytes32[] topics;
        bytes data;
    }

    // readCallers() 的可能调用者模式
    enum CallerMode {
        None,
        Broadcast,
        RecurrentBroadcast,
        Prank,
        RecurrentPrank
    }

    enum AccountAccessKind {
        Call,
        DelegateCall,
        CallCode,
        StaticCall,
        Create,
        SelfDestruct,
        Resume
    }

    struct Wallet {
        address addr;
        uint256 publicKeyX;
        uint256 publicKeyY;
        uint256 privateKey;
    }

    struct ChainInfo {
        uint256 forkId;
        uint256 chainId;
    }

    struct AccountAccess {
        ChainInfo chainInfo;
        AccountAccessKind kind;
        address account;
        address accessor;
        bool initialized;
        uint256 oldBalance;
        uint256 newBalance;
        bytes deployedCode;
        uint256 value;
        bytes data;
        bool reverted;
        StorageAccess[] storageAccesses;
    }

    struct StorageAccess {
        address account;
        bytes32 slot;
        bool isWrite;
        bytes32 previousValue;
        bytes32 newValue;
        bool reverted;
    }

    // 从名称派生私钥，用该名称标记账户，并返回钱包
    function createWallet(string calldata) external returns (Wallet memory);

    // 从私钥生成钱包并返回钱包
    function createWallet(uint256) external returns (Wallet memory);

    // 从私钥生成钱包，用该名称标记账户，并返回钱包
    function createWallet(uint256, string calldata) external returns (Wallet memory);

    // 签名数据，(Wallet, digest) => (v, r, s)
    function sign(Wallet calldata, bytes32) external returns (uint8, bytes32, bytes32);

    // 获取钱包的 nonce
    function getNonce(Wallet calldata) external returns (uint64);

    // 设置 block.timestamp
    function warp(uint256) external;

    // 设置 block.number
    function roll(uint256) external;

    // 设置 block.basefee
    function fee(uint256) external;

    // 设置 block.difficulty
    // 从巴黎硬分叉开始不再起作用，并将回滚。
    function difficulty(uint256) external;
    
    // 设置 block.prevrandao
    // 在巴黎硬分叉之前不起作用，并将回滚。
    function prevrandao(bytes32) external;

    // 设置 block.chainid
    function chainId(uint256) external;

    // 从地址加载存储槽
    function load(address account, bytes32 slot) external returns (bytes32);

    // 将值存储到地址的存储槽
    function store(address account, bytes32 slot, bytes32 value) external;

    // 签名数据
    function sign(uint256 privateKey, bytes32 digest)
        external
        returns (uint8 v, bytes32 r, bytes32 s);

    // 从给定的私钥计算地址
    function addr(uint256 privateKey) external returns (address);

    // 从提供的助记词字符串或助记词文件路径派生私钥，
    // 在派生路径 m/44'/60'/0'/0/{index}。
    function deriveKey(string calldata, uint32) external returns (uint256);
    // 从提供的助记词字符串或助记词文件路径派生私钥，
    // 在派生路径 {path}{index}
    function deriveKey(string calldata, string calldata, uint32) external returns (uint256);

    // 获取账户的 nonce
    function getNonce(address account) external returns (uint64);

    // 设置账户的 nonce
    // 新的 nonce 必须高于账户的当前 nonce
    function setNonce(address account, uint64 nonce) external;

    // 通过终端执行外部函数调用
    function ffi(string[] calldata) external returns (bytes memory);

    // 设置环境变量，(name, value)
    function setEnv(string calldata, string calldata) external;

    // 读取环境变量，(name) => (value)
    function envBool(string calldata) external returns (bool);
    function envUint(string calldata) external returns (uint256);
    function envInt(string calldata) external returns (int256);
    function envAddress(string calldata) external returns (address);
    function envBytes32(string calldata) external returns (bytes32);
    function envString(string calldata) external returns (string memory);
    function envBytes(string calldata) external returns (bytes memory);

    // 读取环境变量作为数组，(name, delim) => (value[])
    function envBool(string calldata, string calldata)
        external
        returns (bool[] memory);
    function envUint(string calldata, string calldata)
        external
        returns (uint256[] memory);
    function envInt(string calldata, string calldata)
        external
        returns (int256[] memory);
    function envAddress(string calldata, string calldata)
        external
        returns (address[] memory);
    function envBytes32(string calldata, string calldata)
        external
        returns (bytes32[] memory);
    function envString(string calldata, string calldata)
        external
        returns (string[] memory);
    function envBytes(string calldata, string calldata)
        external
        returns (bytes[] memory);

    // 读取环境变量并提供默认值，(name, value) => (value)
    function envOr(string calldata, bool) external returns (bool);
    function envOr(string calldata, uint256) external returns (uint256);
    function envOr(string calldata, int256) external returns (int256);
    function envOr(string calldata, address) external returns (address);
    function envOr(string calldata, bytes32) external returns (bytes32);
    function envOr(string calldata, string calldata) external returns (string memory);
    function envOr(string calldata, bytes calldata) external returns (bytes memory);
    
    // 读取环境变量作为数组并提供默认值，(name, value[]) => (value[])
    function envOr(string calldata, string calldata, bool[] calldata) external returns (bool[] memory);
    function envOr(string calldata, string calldata, uint256[] calldata) external returns (uint256[] memory);
    function envOr(string calldata, string calldata, int256[] calldata) external returns (int256[] memory);
    function envOr(string calldata, string calldata, address[] calldata) external returns (address[] memory);
    function envOr(string calldata, string calldata, bytes32[] calldata) external returns (bytes32[] memory);
    function envOr(string calldata, string calldata, string[] calldata) external returns (string[] memory);
    function envOr(string calldata, string calldata, bytes[] calldata) external returns (bytes[] memory);

    // 将 Solidity 类型转换为字符串
    function toString(address) external returns(string memory);
    function toString(bytes calldata) external returns(string memory);
    function toString(bytes32) external returns(string memory);
    function toString(bool) external returns(string memory);
    function toString(uint256) external returns(string memory);
    function toString(int256) external returns(string memory);

    // 设置下一个调用的 msg.sender 为输入地址
    function prank(address) external;

    // 设置所有后续调用的 msg.sender 为输入地址
    // 直到调用 `stopPrank`
    function startPrank(address) external;

    // 设置下一个调用的 msg.sender 为输入地址，
    // 并将 tx.origin 设置为第二个输入
    function prank(address, address) external;

    // 设置所有后续调用的 msg.sender 为输入地址，直到
    // 调用 `stopPrank`，并将 tx.origin 设置为第二个输入
    function startPrank(address, address) external;

    // 重置后续调用的 msg.sender 为 `address(this)`
    function stopPrank() external;

    // 从状态中读取当前的 `msg.sender` 和 `tx.origin`，并报告是否有任何活动的调用者修改
    function readCallers() external returns (CallerMode callerMode, address msgSender, address txOrigin);

    // 设置地址的余额
    function deal(address who, uint256 newBalance) external;
    
    // 设置地址的代码
    function etch(address who, bytes calldata code) external;

    // 标记测试为跳过。必须在测试顶部调用。
    function skip(bool skip) external;

    // 预期下一个调用出现错误
    function expectRevert() external;
    function expectRevert(bytes calldata) external;
    function expectRevert(bytes4) external;

    // 记录所有存储读取和写入
    function record() external;

    // 获取给定地址在记录会话期间访问的所有读取和写入槽，
    function accesses(address)
        external
        returns (bytes32[] memory reads, bytes32[] memory writes);
    
    // 记录作为 CREATE、CALL 或 SELFDESTRUCT 操作码的一部分的所有账户访问，
    // 以及调用的上下文。
    function startStateDiffRecording() external;

    // 返回 `startStateDiffRecording` 会话中的所有账户访问的有序数组。
    function stopAndReturnStateDiff() external returns (AccountAccess[] memory accesses);

    // 记录所有交易日志
    function recordLogs() external;

    // 获取所有记录的日志
    function getRecordedLogs() external returns (Log[] memory);

    // 准备一个预期日志，签名如下：
    //   (bool checkTopic1, bool checkTopic2, bool checkTopic3, bool checkData).
    //
    // 调用此函数，然后发出一个事件，然后调用一个函数。
    // 内部在调用之后，我们检查是否按预期顺序发出了日志
    // 带有预期的主题和数据（由布尔值指定）
    //
    // 第二种形式还检查提供的地址与发出事件的合约。
    function expectEmit(bool, bool, bool, bool) external;
    function expectEmit(bool, bool, bool, bool, address) external;

    // 模拟对地址的调用，返回指定数据。
    //
    // 调用数据可以是严格匹配或部分匹配，例如，如果你只
    // 传递一个 Solidity 选择器到预期的调用数据，那么整个 Solidity
    // 函数将被模拟。
    function mockCall(address, bytes calldata, bytes calldata) external;

    // 模拟对地址的调用，返回指定的错误
    //
    // 调用数据可以是严格匹配或部分匹配，例如，如果你只
    // 传递一个 Solidity 选择器到预期的调用数据，那么整个 Solidity
    // 函数将被模拟。
    function mockCallRevert(address where, bytes calldata data, bytes calldata retdata) external;

    // 清除所有模拟和回滚的模拟调用
    function clearMockedCalls() external;

    // 预期对地址的调用，带有指定的调用数据。
    // 调用数据可以是严格匹配或部分匹配
    function expectCall(address callee, bytes calldata data) external;
    // 预期对地址的调用，带有指定的
    // 调用数据和消息值。
    // 调用数据可以是严格匹配或部分匹配
    function expectCall(address callee, uint256, bytes calldata data) external;

    // 从 artifact 文件获取 _creation_ 字节码。传入 json 文件的相对路径
    function getCode(string calldata) external returns (bytes memory);
    // 从 artifact 文件获取 _deployed_ 字节码。传入 json 文件的相对路径
    function getDeployedCode(string calldata) external returns (bytes memory);

    // 在测试跟踪中标记地址
    function label(address addr, string calldata label) external;
    
    // 检索地址的标签
    function getLabel(address addr) external returns (string memory);

    // 在模糊测试时，如果条件不满足，生成新的输入
    function assume(bool) external;

    // 设置 block.coinbase (who)
    function coinbase(address) external;

    // 使用调用测试合约的地址或提供的地址
    // 作为发送者，使下一个调用（仅在此调用深度）创建一个
    // 可以稍后签名并发送上链的交易
    function broadcast() external;
    function broadcast(address) external;

    // 使用调用测试合约的地址或提供的地址
    // 作为发送者，使所有后续调用（仅在此调用深度）创建
    // 可以稍后签名并发送上链的交易
    function startBroadcast() external;
    function startBroadcast(address) external;
    function startBroadcast(uint256 privateKey) external;

    // 停止收集上链交易
    function stopBroadcast() external;

    // 将文件的全部内容读取为字符串，(path) => (data)
    function readFile(string calldata) external returns (string memory);
    // 获取当前项目根路径
    function projectRoot() external returns (string memory);
    // 将文件的下一行读取为字符串，(path) => (line)
    function readLine(string calldata) external returns (string memory);
    // 将数据写入文件，如果不存在则创建文件，如果存在则完全替换其内容。
    // (path, data) => ()
    function writeFile(string calldata, string calldata) external;
    // 将行写入文件，如果不存在则创建文件。
    // (path, data) => ()
    function writeLine(string calldata, string calldata) external;
    // 关闭文件以读取，重置偏移量并允许从头开始读取。
    // (path) => ()
    function closeFile(string calldata) external;
    // 删除文件。此作弊码将在以下情况下回滚，但不限于这些情况：
    // - 路径指向目录。
    // - 文件不存在。
    // - 用户缺乏删除文件的权限。
    // (path) => ()
    function removeFile(string calldata) external;
    // 如果给定路径指向现有实体，则返回 true，否则返回 false
    // (path) => (bool)
    function exists(string calldata) external returns (bool);
    // 如果路径存在于磁盘上并且指向常规文件，则返回 true，否则返回 false
    // (path) => (bool)
    function isFile(string calldata) external returns (bool);
    // 如果路径存在于磁盘上并且指向目录，则返回 true，否则返回 false
    // (path) => (bool)
    function isDir(string calldata) external returns (bool);
    
    // 返回与 'key' 对应的值
    function parseJson(string memory json, string memory key) external returns (bytes memory);
    // 返回整个 json 文件
    function parseJson(string memory json) external returns (bytes memory);
    // 检查 json 字符串中是否存在键
    function keyExists(string memory json, string memory key) external returns (bytes memory);
    // 获取 json 字符串中的键列表
    function parseJsonKeys(string memory json, string memory key) external returns (string[] memory);

    // 快照当前的 evm 状态。
    // 返回创建的快照的 id。
    // 要恢复快照，请使用 `revertTo`
    function snapshot() external returns (uint256);
    // 将 evm 状态恢复到之前的快照
    // 传入要恢复的快照 id。
    // 这将删除快照以及在此快照 id 之后拍摄的所有快照。
    function revertTo(uint256) external returns (bool);

    // 使用给定的端点和块创建新的分叉，
    // 并返回分叉的标识符
    function createFork(string calldata, uint256) external returns (uint256);
    // 使用给定的端点和 _latest_ 块创建新的分叉，
    // 并返回分叉的标识符
    function createFork(string calldata) external returns (uint256);

    // 创建并选择一个新的分叉，使用给定的端点和块，
    // 并返回分叉的标识符
    function createSelectFork(string calldata, uint256)
        external
        returns (uint256);
    // 创建并选择一个新的分叉，使用给定的端点和
    // 最新块，并返回分叉的标识符
    function createSelectFork(string calldata) external returns (uint256);

    // 使用 `createFork` 创建的分叉标识符
    // 并将相应的分叉状态设置为活动状态。
    function selectFork(uint256) external;

    // 返回当前活动的分叉
    // 如果没有活动的分叉，则回滚
    function activeFork() external returns (uint256);

    // 将当前活动的分叉更新到给定的块号
    // 这类似于 `roll`，但对于当前活动的分叉
    function rollFork(uint256) external;
    // 将给定的分叉更新到给定的块号
    function rollFork(uint256 forkId, uint256 blockNumber) external;

    // 从活动的分叉获取给定的交易并在当前状态下执行
    function transact(bytes32) external;
    // 从给定的分叉获取给定的交易并在当前状态下执行
    function transact(uint256, bytes32) external;

    // 标记账户应在使用多分叉设置时跨分叉使用持久存储，
    // 这意味着对该状态的更改将在切换分叉时保留
    function makePersistent(address) external;
    function makePersistent(address, address) external;
    function makePersistent(address, address, address) external;
    function makePersistent(address[] calldata) external;
    // 撤销之前通过 `makePersistent` 添加的地址的持久状态
    function revokePersistent(address) external;
    function revokePersistent(address[] calldata) external;
    // 如果账户被标记为持久状态，则返回 true
    function isPersistent(address) external returns (bool);

    /// 返回给定别名的 RPC url
    function rpcUrl(string calldata) external returns (string memory);
    /// 返回所有 rpc urls 及其别名 `[alias, url][]`
    function rpcUrls() external returns (string[2][] memory);
}
```
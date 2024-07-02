## anvil

### 名称

anvil - 创建一个本地测试网节点，用于部署和测试智能合约。它还可以用于分叉其他 EVM 兼容网络。

### 概要

`anvil` [*选项*]

### 描述

创建一个本地测试网节点，用于部署和测试智能合约。它还可以用于分叉其他 EVM 兼容网络。

本节涵盖了关于挖矿模式、支持的传输层、支持的 RPC 方法、Anvil 标志及其用法的广泛信息列表。你可以同时运行多个标志。

#### 挖矿模式
挖矿模式指的是使用 Anvil 挖矿的频率。默认情况下，它会在提交交易时自动生成一个新区块。

你可以更改此设置为间隔挖矿，这意味着将在用户选择的给定时间段内生成一个新区块。如果你想要这种类型的挖矿，可以通过添加 `--block-time <block-time-in-seconds>` 标志来实现，如下例所示。
```sh
# 每 10 秒生成一个新区块
anvil --block-time 10
```

还有一种第三种挖矿模式称为从不。在这种情况下，它禁用自动和间隔挖矿，改为按需挖矿。你可以通过输入以下命令来实现：
```sh
# 启用从不挖矿模式
anvil --no-mining
```

为了加快区块的最终确定，你可以使用 `--slots-in-an-epoch` 标志并设置值为 `1`。这将导致高度为 `N-2` 的区块被最终确定，其中 `N` 是最新区块。

#### 支持的传输层
支持 HTTP 和 Websocket 连接。服务器默认监听端口 8545，但可以通过运行以下命令进行更改：

```sh
anvil --port <端口>
```

#### 默认 CREATE2 部署者
Anvil 在未分叉时，包含 [默认 CREATE2 部署者代理](https://github.com/Arachnid/deterministic-deployment-proxy) 在地址 `0x4e59b44847b379578588920ca78fbf26c0b4956c`。

这允许你在本地测试 CREATE2 部署而无需分叉。

#### 支持的 RPC 方法
##### 标准方法
标准方法基于 [此](https://eth.wiki/json-rpc/API) 参考。

* `web3_clientVersion`

* `web3_sha3`

* `eth_chainId`

* `eth_networkId`

* `eth_gasPrice`

* `eth_accounts`

* `eth_blockNumber`

* `eth_getBalance`

* `eth_getStorageAt`

* `eth_getBlockByHash`

* `eth_getBlockByNumber`

* `eth_getTransactionCount`

* `eth_getBlockTransactionCountByHash`

* `eth_getBlockTransactionCountByNumber`

* `eth_getUncleCountByBlockHash`

* `eth_getUncleCountByBlockNumber`

* `eth_getCode`

* `eth_sign`

* `eth_signTypedData_v4`

* `eth_sendTransaction`

* `eth_sendRawTransaction`

* `eth_call`

* `eth_createAccessList`

* `eth_estimateGas`

* `eth_getTransactionByHash`

* `eth_getTransactionByBlockHashAndIndex`

* `eth_getTransactionByBlockNumberAndIndex`

* `eth_getTransactionReceipt`

* `eth_getUncleByBlockHashAndIndex`

* `eth_getUncleByBlockNumberAndIndex`

* `eth_getLogs`

* `eth_newFilter`

* `eth_getFilterChanges`

* `eth_newBlockFilter`

* `eth_newPendingTransactionFilter`

* `eth_getFilterLogs`

* `eth_uninstallFilter`

* `eth_getWork`

* `eth_subscribe`

* `eth_unsubscribe`

* `eth_syncing`

* `eth_submitWork`

* `eth_submitHashrate`

* `eth_feeHistory`

* `eth_getProof`

* `debug_traceTransaction`
使用 `anvil --steps-tracing` 获取 `structLogs`

* `debug_traceCall`
注意，非标准追踪尚未支持。这意味着你不能传递任何参数给 `trace` 参数。

* `trace_transaction`

* `trace_block`

##### 自定义方法
`anvil_*` 命名空间是 `hardhat` 的别名。更多信息，请参考 [Hardhat 文档](https://hardhat.org/hardhat-network/reference#hardhat-network-methods)。

`anvil_impersonateAccount`
发送交易伪装成外部拥有的账户或合约。

`anvil_stopImpersonatingAccount`
停止伪装账户或合约，如果之前通过 `anvil_impersonateAccount` 设置。

`anvil_autoImpersonateAccount`
接受 `true` 启用自动伪装账户，`false` 禁用。启用后，任何交易的 sender 将自动伪装。与 `anvil_impersonateAccount` 相同。

`anvil_getAutomine`
如果启用自动挖矿，返回 true，否则返回 false。

`anvil_mine`
挖一系列区块。

`anvil_dropTransaction`
从池中移除交易。

`anvil_reset`
重置分叉到新的分叉状态，并可选地更新分叉配置。

`anvil_setRpcUrl`
设置后端 RPC URL。

`anvil_setBalance`
修改账户的余额。

`anvil_setCode`
设置合约的代码。

`anvil_setNonce`
设置地址的 nonce。

`anvil_setStorageAt`
写入账户存储的单个槽。

`anvil_setCoinbase`
设置 coinbase 地址。

`anvil_setLoggingEnabled`
启用或禁用日志记录。

`anvil_setMinGasPrice`
设置节点的最低 gas 价格。

`anvil_setNextBlockBaseFeePerGas`
设置下一个区块的基础费用。

`anvil_setChainId`
设置当前 EVM 实例的链 ID。

`anvil_dumpState`
返回表示链完整状态的十六进制字符串。可以重新导入到 Anvil 的新实例中以重新获得相同的状态。

`anvil_loadState`
当给定之前由 `anvil_dumpState` 返回的十六进制字符串时，将内容合并到当前链状态中。将覆盖任何冲突的账户/存储槽。

`anvil_nodeInfo`
检索当前运行的 Anvil 节点的配置参数。

##### 特殊方法
特殊方法来自 Ganache。你可以查看文档 [这里](https://github.com/trufflesuite/ganache-cli-archive/blob/master/README.md)。

`evm_setAutomine`
根据单个布尔参数启用或禁用自动挖矿，即每次向网络提交新交易时自动挖矿。

`evm_setIntervalMining`
将挖矿行为设置为间隔，并设置给定的间隔（秒）。

`evm_snapshot`
在当前区块快照区块链状态。

`evm_revert`
将区块链状态恢复到之前的快照。接受单个参数，即要恢复到的快照 ID。

`evm_increaseTime`
向前跳跃给定的时间量，以秒为单位。

`evm_setNextBlockTimestamp`
类似于 `evm_increaseTime`，但接受下一个区块的确切时间戳。

`anvil_setBlockTimestampInterval`
类似于 `evm_increaseTime`，但设置区块时间戳 `interval`。下一个区块的时间戳将计算为 `lastBlock_timestamp + interval`。

`evm_setBlockGasLimit`
设置后续区块的区块 gas 限制。

`anvil_removeBlockTimestampInterval`
移除存在的 `anvil_setBlockTimestampInterval`。

`evm_mine`
挖一个区块。

`anvil_enableTraces`
为返回给用户的交易开启调用追踪（而不是仅返回 txhash/receipt）。

`eth_sendUnsignedTransaction`
无论签名状态如何，执行交易。

对于接下来的三个方法，请确保阅读 [Geth 的文档](https://geth.ethereum.org/docs/rpc/ns-txpool)。

`txpool_status`
返回当前等待包含在下一个区块中的交易数量，以及仅计划未来执行的交易。

`txpool_inspect`
返回当前等待包含在下一个区块中的所有交易的摘要，以及仅计划未来执行的交易。

`txpool_content`
返回当前等待包含在下一个区块中的所有交易的详细信息，以及仅计划未来执行的交易。

##### Otterscan 方法
`ots_*` 命名空间实现了 [Otterscan 规范](https://github.com/otterscan/otterscan/blob/develop/docs/custom-jsonrpc.md)。

`ots_getApiLevel`
由 Otterscan 用于检查是否连接到兼容节点，并在不兼容时显示友好消息。

`ots_getInternalOperations`
返回交易中的内部 ETH 转账。

`ots_hasCode`
检查某个地址是否包含已部署的代码。

`ots_getTransactionError`
提取交易的原始错误输出。

`ots_traceTransaction`
提取所有类型的调用、合约创建和自毁，并返回调用树。

`ots_getBlockDetails`
为 Otterscan 的区块详情页面定制和扩展的 eth_getBlock*。

`ots_getBlockTransactions`
获取某个区块的分页交易，并移除一些冗余字段如 logs。

`ots_searchTransactionsBefore`
获取某个地址的分页入站/出站交易调用，并在给定目标区块之前。

`ots_searchTransactionsAfter`
获取某个地址的分页入站/出站交易调用，并在给定目标区块之后。

`ots_getTransactionBySenderAndNonce`
获取某个发送者地址的给定 nonce 的交易哈希。

`ots_getContractCreator`
获取创建合约的交易哈希和地址。


### 选项
#### 通用选项
`-a, --accounts <账户数>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置账户数量。[默认: 10]

`--auto-impersonate`
&nbsp;&nbsp;&nbsp;&nbsp; 启动时启用 autoImpersonate。

`-b, --block-time <区块时间>`
&nbsp;&nbsp;&nbsp;&nbsp; 间隔挖矿的区块时间（秒）。

`--balance <余额>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置账户的余额。[默认: 10000]

`--derivation-path <派生路径>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置要派生的子密钥的派生路径。[默认: m/44'/60'/0'/0/]

`-h, --help`
&nbsp;&nbsp;&nbsp;&nbsp; 打印帮助信息。

`--hardfork <硬分叉>`
&nbsp;&nbsp;&nbsp;&nbsp; 选择要使用的 EVM 硬分叉，例如 `shanghai`, `paris`, `london` 等... [默认: latest]

`--init <路径>`
&nbsp;&nbsp;&nbsp;&nbsp; 使用给定的 `genesis.json` 文件初始化创世区块。

`-m, --mnemonic <助记词>`
&nbsp;&nbsp;&nbsp;&nbsp; 用于生成账户的 BIP39 助记词短语。

`--no-mining`
&nbsp;&nbsp;&nbsp;&nbsp; 禁用自动和间隔挖矿，改为按需挖矿。

`--order <排序>`
&nbsp;&nbsp;&nbsp;&nbsp; 交易在 mempool 中的排序方式。[默认: fees]

`-p, --port <端口>`
&nbsp;&nbsp;&nbsp;&nbsp; 监听的端口号。[默认: 8545]

`--steps-tracing`
&nbsp;&nbsp;&nbsp;&nbsp; 启用用于 debug 调用返回 geth 风格追踪的步骤追踪。[别名: tracing]

`--ipc [<路径>]`
&nbsp;&nbsp;&nbsp;&nbsp; 在给定的 `路径` 参数或默认路径启动 IPC 端点：unix: `tmp/anvil.ipc`，windows: `\\.\pipe\anvil.ipc`。

`--silent`
&nbsp;&nbsp;&nbsp;&nbsp; 启动时不打印任何内容。

`--timestamp <时间戳>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置创世区块的时间戳。

`-V, --version`
&nbsp;&nbsp;&nbsp;&nbsp; 打印版本信息。

`--disable-default-create2-deployer`
&nbsp;&nbsp;&nbsp;&nbsp; 禁用在未分叉时运行 Anvil 的默认 CREATE2 工厂部署。

#### EVM 选项
`-f, --fork-url <URL>`
&nbsp;&nbsp;&nbsp;&nbsp; 从远程端点获取状态，而不是从空状态开始。

`--fork-block-number <区块>`
&nbsp;&nbsp;&nbsp;&nbsp; 从远程端点获取特定区块号的状态（必须在同一命令行中传递 `--fork-url`）。

`--fork-retry-backoff <回退>`
&nbsp;&nbsp;&nbsp;&nbsp; 遇到错误时的初始重试回退。

`--retries <重试次数>`
&nbsp;&nbsp;&nbsp;&nbsp; 虚假网络的重试请求次数（超时请求）。[默认: 5]

`--timeout <超时>`
&nbsp;&nbsp;&nbsp;&nbsp; 分叉模式下发送给远程 JSON-RPC 服务器的请求超时（毫秒）。[默认: 45000]

`--compute-units-per-second <CUPS>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置此提供者假设的每秒可用计算单元数。[默认: 330]
&nbsp;&nbsp;&nbsp;&nbsp; 另见，[Alchemy Ratelimits](https://github.com/alchemyplatform/alchemy-docs/blob/master/documentation/compute-units.md#rate-limits-cups)。

`--no-rate-limit`
&nbsp;&nbsp;&nbsp;&nbsp; 禁用此节点的提供者速率限制。如果存在，将始终覆盖 `--compute-units-per-second`。[默认: false]
&nbsp;&nbsp;&nbsp;&nbsp; 另见，[Alchemy Ratelimits](https://github.com/alchemyplatform/alchemy-docs/blob/master/documentation/compute-units.md#rate-limits-cups)。

`--no-storage-caching`
&nbsp;&nbsp;&nbsp;&nbsp; 禁用 RPC 缓存；所有存储槽都从端点读取。此标志覆盖项目的配置文件（必须在同一命令行中传递 --fork-url）。


#### 执行环境配置
`--base-fee <费用>`
`--block-base-fee-per-gas <费用>`
&nbsp;&nbsp;&nbsp;&nbsp; 区块中的基础费用。

`--chain-id <链ID>`
&nbsp;&nbsp;&nbsp;&nbsp; 链 ID。[默认: 31337]

`--code-size-limit <代码大小>`
&nbsp;&nbsp;&nbsp;&nbsp; EIP-170: 合约代码大小限制（字节）。用于增加测试的限制。[默认: 0x6000 (~25kb)]

`--gas-limit <gas限制>`
&nbsp;&nbsp;&nbsp;&nbsp; 区块 gas 限制。

`--gas-price <gas价格>`
&nbsp;&nbsp;&nbsp;&nbsp; gas 价格。

#### 服务器选项
`--allow-origin <允许来源>`
&nbsp;&nbsp;&nbsp;&nbsp; 设置 CORS `allow_origin`。[默认: *]

`--no-cors`
&nbsp;&nbsp;&nbsp;&nbsp; 禁用 CORS。

`--host <主机>`
&nbsp;&nbsp;&nbsp;&nbsp; 服务器将监听的 IP 地址。

`--config-out <输出文件>`
&nbsp;&nbsp;&nbsp;&nbsp; 将 `anvil` 的输出作为 json 写入用户指定的文件。

`--prune-history`
&nbsp;&nbsp;&nbsp;&nbsp; 不保留完整的链历史。

### 示例
1. 设置账户数量为 15 且余额为 300 ETH
  ```sh
  anvil --accounts 15 --balance 300
  ```

2. 选择将执行测试的地址
  ```sh
  anvil --sender 0xC8479C45EE87E0B437c09d3b8FE8ED14ccDa825E
  ```

3. 将交易在 mempool 中的排序方式更改为 FIFO
  ```sh
  anvil --order fifo
  ```

### Shell 补全

``anvil completions`` *shell*

为给定的 shell 生成 shell 补全脚本。

支持的 shell 有：

- bash
- elvish
- fish
- powershell
- zsh

#### 示例

1. 为 zsh 生成 shell 补全脚本：
    ```sh
    anvil completions zsh > $HOME/.oh-my-zsh/completions/_anvil
    ```


### 在 Docker 中使用

为了在 Github Actions 中以 [Docker 容器](../../tutorials/foundry-docker.md) 运行 anvil 作为服务，其中无法向 entrypoint 命令传递参数，使用 `ANVIL_IP_ADDR` 环境变量设置主机的 IP。`ANVIL_IP_ADDR=0.0.0.0` 等同于提供 `--host <ip>` 选项。

#### 使用 `genesis.json`

Anvil 中的 `genesis.json` 文件与 Geth 中的作用类似，定义网络的初始状态、共识规则和预分配账户，以确保所有节点一致启动并维护网络完整性。所有值，包括余额、gas 限制等，均定义为十六进制。

- `chainId`: 区块链的标识符，每个网络唯一。
- `nonce`: 用于确保数据完整性的哈希算法中的计数器。
- `timestamp`: 创世区块的创建时间，Unix 时间。
- `extraData`: 创世区块创建者可以包含的额外数据。
- `gasLimit`: 区块中可以使用的最大 gas 量。
- `difficulty`: 挖矿新区块的难度。
- `mixHash`: 证明区块有足够计算量的唯一标识符。
- `coinbase`: 挖出此区块的矿工的以太坊地址。
- `stateRoot`: 状态 trie 的根，反映所有交易后的最终状态。
- `alloc`: 允许预分配以太币到一组具有预定义余额的地址。
- `number`: 区块号，创世区块为 0。
- `gasUsed`: 区块中使用的总 gas。
- `parentHash`: 父区块的哈希，创世区块为全零，因为没有父区块。

模拟主网的 genesis 示例可以在这里找到 [这里](https://github.com/paradigmxyz/reth/blob/8f3e4a15738d8174d41f4aede5570ecead141a77/crates/primitives/res/genesis/mainnet.json)。

```json
{
  "chainId": "0x2323",
  "nonce": "0x42",
  "timestamp": "0x0",
  "extraData": "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa",
  "gasLimit": "0x1388",
  "difficulty": "0x400000000",
  "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "stateRoot": "0xd7f8974fb5ac78d9ac099b9ad5018bedc2ce0a72dad1827a1709da30580f0544",
  "alloc": {
    "000d836201318ec6899a67540690382780743280": {
      "balance": "0xad78ebc5ac6200000"
    }
  },
  "number": "0x0",
  "gasUsed": "0x0",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```
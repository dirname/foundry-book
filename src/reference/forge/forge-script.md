## forge script

### 名称

forge-script - 将智能合约作为脚本运行，构建可以发送到链上的交易。

### 概要

``forge script`` [*选项*] *路径* [*参数...*]

### 描述

将智能合约作为脚本运行，构建可以发送到链上的交易。

脚本可以用于在实时合约上应用状态转换，或者使用 Solidity 部署和初始化一组复杂的智能合约。

### 选项

`--broadcast`  
&nbsp;&nbsp;&nbsp;&nbsp;广播交易。

`--debug`  
&nbsp;&nbsp;&nbsp;&nbsp;在 [调试器][debugger] 中打开脚本。优先于广播。

`-g`  
`--gas-estimate-multiplier` *乘数*  
&nbsp;&nbsp;&nbsp;&nbsp;按相对百分比乘以所有 gas 估计值。（例如，设置为 200 以加倍它们）
&nbsp;&nbsp;&nbsp;&nbsp;默认值：130

`--json`  
&nbsp;&nbsp;&nbsp;&nbsp;以 JSON 格式输出结果。  
&nbsp;&nbsp;&nbsp;&nbsp;注意：输出正在开发中，容易发生变化。

`--legacy`  
&nbsp;&nbsp;&nbsp;&nbsp;使用传统交易而不是 EIP1559 交易。对于没有 EIP1559 的常见网络会自动启用。

`--resume`  
&nbsp;&nbsp;&nbsp;&nbsp;恢复提交之前失败或超时的交易。

`-s`  
`--sig` *签名*  
&nbsp;&nbsp;&nbsp;&nbsp;你想要在合约中调用的函数的签名，或原始 calldata。  
&nbsp;&nbsp;&nbsp;&nbsp;默认值：`run()`  

`--skip-simulation`  
&nbsp;&nbsp;&nbsp;&nbsp;跳过链上模拟。

`--skip`  
&nbsp;&nbsp;&nbsp;&nbsp;跳过非必要合约目录的编译，如 test 或 script（用法 `--skip test`）。

`--non-interactive`  
&nbsp;&nbsp;&nbsp;&nbsp;移除如果合约接近 [EIP-170](https://eips.ethereum.org/EIPS/eip-170) 大小限制时出现的交互提示。

`--slow`  
&nbsp;&nbsp;&nbsp;&nbsp;确保交易在之前的交易被确认并成功后才发送。

`--target-contract` *合约名称*  
&nbsp;&nbsp;&nbsp;&nbsp;你想要运行的合约的名称。

`--priority-gas-price`  
&nbsp;&nbsp;&nbsp;&nbsp;设置 EIP1559 交易的优先 gas 价格。在 gas 价格波动时，用于确保交易被包含。

`--with-gas-price` *价格*  
&nbsp;&nbsp;&nbsp;&nbsp;设置 **广播** 传统交易的 gas 价格，或广播 EIP1559 交易的最大费用。  
&nbsp;&nbsp;&nbsp;&nbsp;注意：要在脚本执行环境中设置 gas 价格，请使用 `--gas-price` 代替（见下文）。

{{#include ../common/etherscan-options.md}}

#### 验证选项

`--verify`  
&nbsp;&nbsp;&nbsp;&nbsp;如果找到匹配的广播日志，尝试验证收据中找到的每个合约。

{{#include ../common/verifier-options.md}}

{{#include ../common/retry-options.md}}

{{#include core-build-options.md}}

#### 构建选项

`--names`  
&nbsp;&nbsp;&nbsp;&nbsp;打印编译的合约名称。

`--sizes`  
&nbsp;&nbsp;&nbsp;&nbsp;打印编译的非测试合约大小，如果任何合约超过大小限制，则退出并返回代码 1。

{{#include watch-options.md}}

{{#include ../common/multi-wallet-options.md}}

{{#include evm-options.md}}

{{#include executor-options.md}}

{{#include common-options.md}}

### 示例

1. 将 `BroadcastTest` 作为脚本运行，在链上广播生成的交易
    ```sh
    forge script ./test/Broadcast.t.sol --tc BroadcastTest --sig "deploy()" \
        -vvv --fork-url $SEPOLIA_RPC_URL
    ```

2. 在 Polygon 上部署合约（[参见脚本教程中的示例脚本](../../tutorials/solidity-scripting.md)）。*每个网络的验证器 URL 不同。*
    ```sh
    forge script script/NFT.s.sol:MyScript --chain-id 137 --rpc-url $RPC_URL \
        --etherscan-api-key $POLYGONSCAN_API_KEY --verifier-url https://api.polygonscan.com/api \
        --broadcast --verify -vvvv
    ```

3. 恢复失败的脚本。以上述为例，移除 `--broadcast` 添加 `--resume`
    ```sh
    forge script script/NFT.s.sol:MyScript --chain-id 137 --rpc-url $RPC_URL \
        --etherscan-api-key $POLYGONSCAN_API_KEY --verifier-url https://api.polygonscan.com/api \
        --verify -vvvv --resume
    ```

4. 验证刚刚使用脚本部署的合约
    ```sh
    forge script script/NFT.s.sol --rpc-url $RPC_URL --verify --resume
    ```

[debugger]: ../../forge/debugger.md
## forge create

### NAME

forge-create - 部署智能合约。

### SYNOPSIS

``forge create`` [*options*] *contract*

### DESCRIPTION

部署智能合约。

合约路径的格式为 `<path>:<contract>`，例如 `src/Contract.sol:Contract`。

你可以使用 `--constructor-args` 指定构造函数参数。或者，你可以使用 `--constructor-args-path` 指定包含空格分隔的构造函数参数的文件。

不支持动态链接：你应该预先部署你的库，并手动指定它们的地址（见 `--libraries`）。

> ℹ️ **注意**
>
> `--constructor-args` 标志必须放在命令的最后，因为它接受多个值。

### OPTIONS

#### Build Options

`--constructor-args` *args...*  
&nbsp;&nbsp;&nbsp;&nbsp;构造函数参数。

`--constructor-args-path` *file*  
&nbsp;&nbsp;&nbsp;&nbsp;包含构造函数参数的文件路径。

`--verify`  
&nbsp;&nbsp;&nbsp;&nbsp;创建后验证合约。运行 `forge verify-contract` 并带有适当的参数。

{{#include ../common/verifier-options.md}}

`--unlocked`  
&nbsp;&nbsp;&nbsp;&nbsp;通过 `eth_sendTransaction` 发送，使用 `--from` 参数或 `$ETH_FROM` 作为发送者。

{{#include ../common/transaction-options.md}}

{{#include ../common/wallet-options.md}}

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

{{#include core-build-options.md}}

{{#include ../common/display-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 部署没有构造函数参数的合约：
    ```sh
    forge create src/Contract.sol:ContractWithNoConstructor
    ```

2. 部署带有两个构造函数参数的合约：
    ```sh
    forge create src/Contract.sol:MyToken --constructor-args "My Token" "MT"
    ```

### SEE ALSO

[forge](./forge.md), [forge build](./forge-build.md), [forge verify-contract](./forge-verify-contract.md)

[eip1559]: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md
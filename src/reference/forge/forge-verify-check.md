## forge verify-check

### NAME

forge-verify-check - 检查所选验证提供商的验证状态。

### SYNOPSIS

``forge verify-check`` [*options*] *id* [*etherscan_key*]

*id* 是验证标识符。对于 Etherscan 和 Bloxroute - 它是提交 GUID，对于 Sourcify - 它是合约地址。

### DESCRIPTION

检查所选验证提供商的验证状态。

对于 Etherscan，您必须提供一个 Etherscan API 密钥，可以通过作为参数传递或设置 `ETHERSCAN_API_KEY`。

### OPTIONS

#### 验证合约选项

{{#include ../common/verifier-options.md}}

`--chain-id` *chain_id*  
&nbsp;&nbsp;&nbsp;&nbsp;合约部署到的链 ID（可以是数字或链名称）。  
&nbsp;&nbsp;&nbsp;&nbsp;默认值：mainnet

{{#include ../common/retry-options.md}}

{{#include common-options.md}}

### SEE ALSO

[forge](./forge.md), [forge create](./forge-create.md), [forge verify-contract](./forge-verify-contract.md)
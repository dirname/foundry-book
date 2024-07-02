## cast estimate

### NAME

cast-estimate - 估算交易所需的 gas 费用。

### SYNOPSIS

``cast estimate`` [*options*] *to* *sig* [*args...*]

### DESCRIPTION

估算交易所需的 gas 费用。

目标地址（*to*）可以是 ENS 名称或地址。

{{#include sig-description.md}}

### OPTIONS

#### 交易选项

{{#include ../common/tx-value-option.md}}

{{#include ../common/rpc-options.md}}

{{#include ../common/etherscan-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 估算调用 WETH 合约上的 `deposit()` 方法所需的 gas 费用：
    ```sh
    cast estimate 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 \
      --value 0.1ether "deposit()"
    ```

### SEE ALSO

[cast](./cast.md), [cast send](./cast-send.md), [cast publish](./cast-publish.md), [cast receipt](./cast-receipt.md)
```markdown
#### 交易选项

`--gas-limit` *gas_limit*  
&nbsp;&nbsp;&nbsp;&nbsp;交易的 gas 限制。

`--gas-price` *price*  
&nbsp;&nbsp;&nbsp;&nbsp;交易的 gas 价格，或 EIP1559 交易的每 gas 最大费用。

`--priority-gas-price` *price*  
&nbsp;&nbsp;&nbsp;&nbsp;EIP1559 交易的每 gas 最大优先费用。

{{#include ../common/tx-value-option.md}}

`--nonce` *nonce*  
&nbsp;&nbsp;&nbsp;&nbsp;交易的 nonce。

`--legacy`  
&nbsp;&nbsp;&nbsp;&nbsp;发送一个传统交易，而不是 [EIP1559][eip1559] 交易。

&nbsp;&nbsp;&nbsp;&nbsp;对于没有 EIP1559 的常见网络，这会自动启用。

[eip1559]: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md
```
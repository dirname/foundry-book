## `pauseGasMetering`

### 签名

```solidity
function pauseGasMetering() external;
```

### 描述

暂停 gas 计量（即 `gasleft()` 不会随着操作的执行而减少）。

这对于通过关闭不必要的代码的 gas 计量来更好地了解 gas 成本非常有用，还可以用于避免长时间运行的脚本因 gas 耗尽而终止。

> ℹ️ **注意**
>
> `pauseGasMetering` *关闭了来自 gas 计量的 DoS 保护*。
>
> 如果服务假设特定实例的 EVM 会因 gas 使用而完成，则不再成立，在这种情况下应启用超时。
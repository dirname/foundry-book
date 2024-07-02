## `sig`

### 签名

```solidity
function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage);
```

```solidity
function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage);
```

### 描述

设置静态调用的函数 4 字节选择器。

默认值：`hex"00000000"`

### 示例

```solidity
uint256 slot = stdstore
    .target(addr)
    .sig(addr.fun.selector)
    .with_key(1)
    .find();
```

```solidity
uint256 slot = stdstore
    .target(addr)
    .sig("fun(uint256)")
    .with_key(1)
    .find();
```
## `with_key`

### 签名

```solidity
function with_key(StdStorage storage self, address who) internal returns (StdStorage storage);
```

```solidity
function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage);
```

```solidity
function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage);
```

### 描述

传递一个参数给函数。

可以多次使用以传递多个参数。顺序很重要。

### 示例

```solidity
uint256 slot = stdstore
    .target(addr)
    .sig("fun(uint256,address)")
    .with_key(1)
    .with_key(address(this))
    .find();
```
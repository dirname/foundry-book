## `deal`

### 签名

```solidity
function deal(address to, uint256 give) public;
```

```solidity
function deal(address token, address to, uint256 give) public;
```

```solidity
function deal(address token, address to, uint256 give, bool adjust) public;
```

### 描述

这是 [`deal`](../../cheatcodes/deal.md) cheatcode 的一个封装，也适用于大多数 ERC-20 代币。

如果使用 `deal` 的替代签名，则在设置余额后调整代币的总供应量。

### 示例

```solidity
deal(address(dai), alice, 10000e18);
assertEq(dai.balanceOf(alice), 10000e18);
```
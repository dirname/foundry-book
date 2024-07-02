## `startHoax`

### 签名

```solidity
function startHoax(address who) public;
```

```solidity
function startHoax(address who, uint256 give) public;
```

```solidity
function startHoax(address who, address origin) public;
```

```solidity
function startHoax(address who, address origin, uint256 give) public;
```

### 描述

从一个有以太币的地址开始一个[永久的 `prank`](../../cheatcodes/start-prank.md)。

如果没有指定余额，它将被设置为 2^128 wei。
## `hoax`

### 签名

```solidity
function hoax(address who) public;
```

```solidity
function hoax(address who, uint256 give) public;
```

```solidity
function hoax(address who, address origin) public;
```

```solidity
function hoax(address who, address origin, uint256 give) public;
```

### 描述

设置一个从拥有一些以太币的地址发起的[`prank`](../../cheatcodes/prank.md)。

如果没有指定余额，它将被设置为 2^128 wei。
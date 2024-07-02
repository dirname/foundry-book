## `makeAddrAndKey`

### 签名

```solidity
function makeAddrAndKey(string memory name) internal returns(address addr, uint256 privateKey);
```

### 描述

从提供的 `name` 创建一个地址和私钥。

为派生的地址创建一个 [`label`](../../cheatcodes/label.md)，使用提供的 `name` 作为标签值。

### 示例

```solidity
(address alice, uint256 key) = makeAddrAndKey("alice");
emit log_address(alice); // 0x328809bc894f92807417d2dad6b7c998c1afdac6
emit log_uint(key); // 70564938991660933374592024341600875602376452319261984317470407481576058979585
```
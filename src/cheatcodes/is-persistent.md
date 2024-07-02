## `isPersistent`

### 签名

```solidity
function isPersistent(address) external returns (bool);
```

### 描述

返回一个账户是否被标记为持久化（[`makePersistent`](./make-persistent.md)）。

### 示例

检查 `msg.sender` 和当前测试账户的默认状态

```solidity
// 默认情况下，`sender` 和测试合约本身是持久化的
assert(cheats.isPersistent(msg.sender));
assert(cheats.isPersistent(address(this)));
```

### 另请参阅

- [makePersistent](./make-persistent.md)
- [revokePersistent](./revoke-persistent.md)
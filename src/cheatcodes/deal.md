## `deal`

### 签名

```solidity
function deal(address who, uint256 newBalance) external;
```

### 描述

将地址 `who` 的余额设置为 `newBalance`。

如果使用 `deal` 的替代签名（定义在 `StdCheats.sol` 中），则可以额外指定 ERC20 代币地址，以及一个更新 `totalSupply` 的选项。

### 示例

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
vm.deal(alice, 1 ether);
log_uint256(alice.balance); // 1000000000000000000
```

```solidity
address alice = makeAddr("alice");
emit log_address(alice);
deal(address(DAI), alice, 1 ether); // 首先导入 StdUtils.sol
log_uint256(address(DAI).balanceOf(alice)); // 1000000000000000000
```

### 参见

Forge 标准库

[`deal`](../reference/forge-std/deal.md), [`hoax`](../reference/forge-std/hoax.md), [`startHoax`](../reference/forge-std/startHoax.md)
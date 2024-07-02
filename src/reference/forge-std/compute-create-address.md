## `computeCreateAddress`

### 签名

```solidity
function computeCreateAddress(address deployer, uint256 nonce) internal pure returns (address)
```

### 描述

计算给定部署者地址和随机数的合约部署地址。这对于预先计算合约将部署的地址非常有用。

### 示例

```solidity
address governanceAddress = computeCreateAddress(0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266, 1);

// 这个合约需要一个尚未部署的治理合约
Contract contract = new Contract(governanceAddress);
// 现在我们部署它
Governance governance = new Governance(contract);

// 假设 `contract` 有一个 `governance()` 访问器
assertEq(governanceAddress, address(governance)); // [PASS]
```
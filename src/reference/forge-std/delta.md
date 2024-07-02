## `delta`

### 签名

```solidity
function delta(uint256 a, uint256 b) internal pure returns (uint256)
```

```solidity
function delta(int256 a, int256 b) internal pure returns (uint256)
```

### 描述

返回两个数字之间的绝对值差。

### 示例

```solidity
uint256 four = stdMath.delta(-1, 3);
```
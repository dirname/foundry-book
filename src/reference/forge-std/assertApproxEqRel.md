## `assertApproxEqRel`

### 签名

```solidity
function assertApproxEqRel(uint256 left, uint256 right, uint256 maxPercentDelta) internal;
```

```solidity
function assertApproxEqRel(uint256 left, uint256 right, uint256 maxPercentDelta, string memory err) internal;
```

```solidity
function assertApproxEqRel(int256 left, int256 right, uint256 maxPercentDelta) internal;
```

```solidity
function assertApproxEqRel(int256 left, int256 right, uint256 maxPercentDelta, string memory err) internal;
```

### 描述

断言 `left` 大约等于 `right`，允许的百分比偏差为 `maxPercentDelta`，其中 `1e18` 表示 100%。

可选地包含一个错误信息在回滚字符串中。

### 示例

```solidity
function testFail () external {
    uint256 a = 100;
    uint256 b = 200;
    assertApproxEqRel(a, b, 0.4e18);
}
```

```ignore
[PASS] testFail() (gas: 23884)
Logs:
  Error: a ~= b not satisfied [uint]
      Expected: 200
        Actual: 100
   Max % Delta: 0.400000000000000000
       % Delta: 0.500000000000000000
```

### 另请参阅

- [`assertApproxEqRelDecimal`](./assertApproxEqRelDecimal.md)
- [`assertApproxEqAbs`](./assertApproxEqAbs.md)
## DSTest 参考

Dappsys Test（简称 DSTest）提供了基本的日志记录和断言功能。它包含在 Forge 标准库中。

要访问这些功能，请导入 `forge-std/Test.sol` 并在你的测试合约中继承 `Test`：

```solidity
import "forge-std/Test.sol";

contract ContractTest is Test {
    // ... 测试 ...
}
```

### 日志记录

以下是所有可用日志记录事件的完整概述。详细描述和示例用法请参见下文。

```solidity
event log                    (string);
event logs                   (bytes);

event log_address            (address);
event log_bytes32            (bytes32);
event log_int                (int);
event log_uint               (uint);
event log_bytes              (bytes);
event log_string             (string);

event log_named_address      (string key, address val);
event log_named_bytes32      (string key, bytes32 val);
event log_named_decimal_int  (string key, int val, uint decimals);
event log_named_decimal_uint (string key, uint val, uint decimals);
event log_named_int          (string key, int val);
event log_named_uint         (string key, uint val);
event log_named_bytes        (string key, bytes val);
event log_named_string       (string key, string val);
```

### 日志记录事件

本节记录所有日志记录事件并提供使用示例。

#### `log`

```solidity
event log(string);
```

##### 示例

```solidity
emit log("here");
// here
```

<br>

---

#### `logs`

```solidity
event logs(bytes);
```

##### 示例

```solidity
emit logs(bytes("abcd"));
// 0x6162636400000000000000000000000000000000000000000000000000000000
```

<br>

---

#### `log_<type>`

```solidity
event log_<type>(<type>);
```

其中 `<type>` 可以是 `address`, `bytes32`, `int`, `uint`, `bytes`, `string`

##### 示例

```solidity
uint256 amount = 1 ether;
emit log_uint(amount);
// 1000000000000000000
```

<br>

---

#### `log_named_<type>`

```solidity
event log_named_<type>(string key, <type> val);
```

其中 `<type>` 可以是 `address`, `bytes32`, `int`, `uint`, `bytes`, `string`

##### 示例

```solidity
uint256 amount = 1 ether;
emit log_named_uint("Amount", amount);
// amount: 1000000000000000000
```

<br>

---

#### `log_named_decimal_<type>`

```solidity
event log_named_decimal_<type>(string key, <type> val, uint decimals);
```

其中 `<type>` 可以是 `int`, `uint`

##### 示例

```solidity
uint256 amount = 1 ether;
emit log_named_decimal_uint("Amount", amount, 18);
// amount: 1.000000000000000000
```

### 断言

以下是所有可用断言函数的完整概述。详细描述和示例用法请参见下文。

```solidity
// 断言 `condition` 为真
function assertTrue(bool condition) internal;
function assertTrue(bool condition, string memory err) internal;

// 断言 `a` 等于 `b`
function assertEq(address a, address b) internal;
function assertEq(address a, address b, string memory err) internal;
function assertEq(bytes32 a, bytes32 b) internal;
function assertEq(bytes32 a, bytes32 b, string memory err) internal;
function assertEq(int a, int b) internal;
function assertEq(int a, int b, string memory err) internal;
function assertEq(uint a, uint b) internal;
function assertEq(uint a, uint b, string memory err) internal;
function assertEqDecimal(int a, int b, uint decimals) internal;
function assertEqDecimal(int a, int b, uint decimals, string memory err) internal;
function assertEqDecimal(uint a, uint b, uint decimals) internal;
function assertEqDecimal(uint a, uint b, uint decimals, string memory err) internal;
function assertEq(string memory a, string memory b) internal;
function assertEq(string memory a, string memory b, string memory err) internal;
function assertEq32(bytes32 a, bytes32 b) internal;
function assertEq32(bytes32 a, bytes32 b, string memory err) internal;
function assertEq0(bytes memory a, bytes memory b) internal;
function assertEq0(bytes memory a, bytes memory b, string memory err) internal;

// 断言 `a` 大于 `b`
function assertGt(uint a, uint b) internal;
function assertGt(uint a, uint b, string memory err) internal;
function assertGt(int a, int b) internal;
function assertGt(int a, int b, string memory err) internal;
function assertGtDecimal(int a, int b, uint decimals) internal;
function assertGtDecimal(int a, int b, uint decimals, string memory err) internal;
function assertGtDecimal(uint a, uint b, uint decimals) internal;
function assertGtDecimal(uint a, uint b, uint decimals, string memory err) internal;

// 断言 `a` 大于或等于 `b`
function assertGe(uint a, uint b) internal;
function assertGe(uint a, uint b, string memory err) internal;
function assertGe(int a, int b) internal;
function assertGe(int a, int b, string memory err) internal;
function assertGeDecimal(int a, int b, uint decimals) internal;
function assertGeDecimal(int a, int b, uint decimals, string memory err) internal;
function assertGeDecimal(uint a, uint b, uint decimals) internal;
function assertGeDecimal(uint a, uint b, uint decimals, string memory err) internal;

// 断言 `a` 小于 `b`
function assertLt(uint a, uint b) internal;
function assertLt(uint a, uint b, string memory err) internal;
function assertLt(int a, int b) internal;
function assertLt(int a, int b, string memory err) internal;
function assertLtDecimal(int a, int b, uint decimals) internal;
function assertLtDecimal(int a, int b, uint decimals, string memory err) internal;
function assertLtDecimal(uint a, uint b, uint decimals) internal;
function assertLtDecimal(uint a, uint b, uint decimals, string memory err) internal;

// 断言 `a` 小于或等于 `b`
function assertLe(uint a, uint b) internal;
function assertLe(uint a, uint b, string memory err) internal;
function assertLe(int a, int b) internal;
function assertLe(int a, int b, string memory err) internal;
function assertLeDecimal(int a, int b, uint decimals) internal;
function assertLeDecimal(int a, int b, uint decimals, string memory err) internal;
function assertLeDecimal(uint a, uint b, uint decimals) internal;
function assertLeDecimal(uint a, uint b, uint decimals, string memory err) internal;

// 断言 `a` 近似等于 `b`，绝对值误差为 `maxDelta`
function assertApproxEqAbs(uint256 a, uint256 b, uint256 maxDelta) internal;
function assertApproxEqAbs(uint256 a, uint256 b, uint256 maxDelta, string memory err) internal;

// 断言 `a` 近似等于 `b`，百分比误差为 `maxPercentDelta`，其中 `1e18` 为 100%
function assertApproxEqRel(uint256 a, uint256 b, uint256 maxPercentDelta) internal;
function assertApproxEqRel(uint256 a, uint256 b, uint256 maxPercentDelta, string memory err) internal;
```

### 断言函数

本节记录所有断言函数并提供使用示例。

#### `assertTrue`

```solidity
function assertTrue(bool condition) internal;
```

断言 `condition` 为真。

##### 示例

```solidity
bool success = contract.fun();
assertTrue(success);
```

<br>

---

#### `assertEq`

```solidity
function assertEq(<type> a, <type> b) internal;
```

其中 `<type>` 可以是 `address`, `bytes32`, `int`, `uint`

断言 `a` 等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertEq(a, b);
```

<br>

---

#### `assertEqDecimal`

```solidity
function assertEqDecimal(<type> a, <type> b, uint decimals) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertEqDecimal(a, b, 18);
```

<br>

---

#### `assertEq32`

```solidity
function assertEq32(bytes32 a, bytes32 b) internal;
```

断言 `a` 等于 `b`。

##### 示例

```solidity
assertEq(bytes32("abcd"), 0x6162636400000000000000000000000000000000000000000000000000000000);
```

<br>

---

#### `assertEq0`

```solidity
function assertEq0(bytes a, bytes b) internal;
```

断言 `a` 等于 `b`。

##### 示例

```solidity
string memory name1 = "Alice";
string memory name2 = "Bob";
assertEq0(bytes(name1), bytes(name2)); // [FAIL]
```

<br>

---

#### `assertGt`

```solidity
function assertGt(<type> a, <type> b) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 大于 `b`。

##### 示例

```solidity
uint256 a = 2 ether;
uint256 b = 1e18 wei;
assertGt(a, b);
```

<br>

---

#### `assertGtDecimal`

```solidity
function assertGtDecimal(<type> a, <type> b, uint decimals) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 大于 `b`。

##### 示例

```solidity
uint256 a = 2 ether;
uint256 b = 1e18 wei;
assertGtDecimal(a, b, 18);
```

<br>

---

#### `assertGe`

```solidity
function assertGe(<type> a, <type> b) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 大于或等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertGe(a, b);
```

<br>

---

#### `assertGeDecimal`

```solidity
function assertGeDecimal(<type> a, <type> b, uint decimals) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 大于或等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertGeDecimal(a, b, 18);
```

<br>

---

#### `assertLt`

```solidity
function assertLt(<type> a, <type> b) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 小于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 2e18 wei;
assertLt(a, b);
```

<br>

---

#### `assertLtDecimal`

```solidity
function assertLtDecimal(<type> a, <type> b, uint decimals) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 小于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 2e18 wei;
assertLtDecimal(a, b, 18);
```

<br>

---

#### `assertLe`

```solidity
function assertLe(<type> a, <type> b) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 小于或等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertLe(a, b);
```

<br>

---

#### `assertLeDecimal`

```solidity
function assertLeDecimal(<type> a, <type> b, uint decimals) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 小于或等于 `b`。

##### 示例

```solidity
uint256 a = 1 ether;
uint256 b = 1e18 wei;
assertLeDecimal(a, b, 18);
```

<br>

---

#### `assertApproxEqAbs`

```solidity
function assertApproxEqAbs(<type> a, <type> b, uint256 maxDelta) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 近似等于 `b`，绝对值误差为 `maxDelta`。

##### 示例

```solidity
function testFail () external {
    uint256 a = 100;
    uint256 b = 200;

    assertApproxEqAbs(a, b, 90);
}
```

<br>

---

#### `assertApproxEqRel`

```solidity
function assertApproxEqRel(<type> a, <type> b, uint256 maxPercentDelta) internal;
```

其中 `<type>` 可以是 `int`, `uint`

断言 `a` 近似等于 `b`，百分比误差为 `maxPercentDelta`，其中 `1e18` 为 100%。

##### 示例

```solidity
function testFail () external {
    uint256 a = 100;
    uint256 b = 200;
    assertApproxEqRel(a, b, 0.4e18);
}
```
<br>

> ℹ️ **信息**
>
> 你可以通过提供额外的参数 `string err` 为上述函数传递自定义错误消息。
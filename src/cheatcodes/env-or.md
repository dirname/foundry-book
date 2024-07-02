## `envOr`

### 签名

```solidity
function envOr(string calldata key, bool defaultValue) external returns (bool value);
function envOr(string calldata key, uint256 defaultValue) external returns (uint256 value);
function envOr(string calldata key, int256 defaultValue) external returns (int256 value);
function envOr(string calldata key, address defaultValue) external returns (address value);
function envOr(string calldata key, bytes32 defaultValue) external returns (bytes32 value);
function envOr(string calldata key, string calldata defaultValue) external returns (string memory value);
function envOr(string calldata key, bytes calldata defaultValue) external returns (bytes memory value);
```

```solidity
function envOr(string calldata key, string calldata delimiter, bool[] calldata defaultValue) external returns (bool[] memory value);
function envOr(string calldata key, string calldata delimiter, uint256[] calldata defaultValue) external returns (uint256[] memory value);
function envOr(string calldata key, string calldata delimiter, int256[] calldata defaultValue) external returns (int256[] memory value);
function envOr(string calldata key, string calldata delimiter, address[] calldata defaultValue) external returns (address[] memory value);
function envOr(string calldata key, string calldata delimiter, bytes32[] calldata defaultValue) external returns (bytes32[] memory value);
function envOr(string calldata key, string calldata delimiter, string[] calldata defaultValue) external returns (string[] memory value);
function envOr(string calldata key, string calldata delimiter, bytes[] calldata defaultValue) external returns (bytes[] memory value);
```

### 描述

一种非失败的方式来读取任何类型的环境变量：如果请求的环境键不存在，`envOr()` 将返回默认值而不是回滚（也适用于数组）。

返回类型由传入的 `defaultValue` 参数的类型决定。

### 提示

- 使用 `envOr(key, defaultValue)` 来读取单个值
- 使用 `envOr(key, delimiter, defaultValue[])` 来读取带有分隔符的数组
- 环境变量的解析将根据 `defaultValue` 的类型进行（例如，如果默认值类型是 `uint`，环境变量也将被解析为 `uint`）
- 对字面量使用显式转换来指定默认变量的类型：`uint(69)` 将返回一个 `uint`，但 `int(69)` 将返回一个 `int`
- 同样适用于：`string("")` 和 `bytes("")` - 这些将分别返回 `string` 和 `bytes`
- 提供默认值时使用动态数组（`bool[]`）而不是固定大小的数组（`bool[4]`）（仅支持动态数组）

### 示例

#### 单个值
如果环境变量 `FORK` 未设置，可以指定其默认值为 `false`：
```solidity
bool fork = vm.envOr("FORK", false);
```
或
```solidity
address owner;

function setUp() {
  owner = vm.envOr("OWNER", address(this));
}
```

#### 数组
如果环境变量 `BAD_TOKENS` 未设置，可以指定默认值为空数组：
```solidity
address[] badTokens;

function envBadTokens() public {
  badTokens = vm.envOr("BAD_TOKENS", ",", badTokens);
}
```
或
```solidity
function envBadTokens() public {
  address[] memory defaultBadTokens = new address[](0);
  address[] memory badTokens = vm.envOr("BAD_TOKENS", ",", defaultBadTokens);
}
```
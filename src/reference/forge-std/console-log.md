## 控制台日志

- 类似于 Hardhat 的控制台函数。
- 您可以在调用和交易中使用它。它适用于视图函数，但不适用于纯函数。
- 无论调用或交易是失败还是成功，它总是有效。
- 要使用它，您需要导入它：
    - `import "forge-std/console.sol";`
- 您可以调用 console.log 最多带 4 个参数，参数可以是以下任意类型：
    - `uint`
    - `string`
    - `bool`
    - `address`
- 还有针对上述类型以及 bytes, bytes1... 到 bytes32 的单参数 API：
    - `console.logInt(int i)`
    - `console.logUint(uint i)`
    - `console.logString(string memory s)`
    - `console.logBool(bool b)`
    - `console.logAddress(address a)`
    - `console.logBytes(bytes memory b)`
    - `console.logBytes1(bytes1 b)`
    - `console.logBytes2(bytes2 b)`
    - ...
    - `console.logBytes32(bytes32 b)`
- console.log 实现了与 Hardhat 的 console.log 相同的格式化选项。
    - 示例：`console.log("Changing owner from %s to %s", currentOwner, newOwner)`
- console.log 是用标准 Solidity 实现的，并且兼容 Anvil 和 Hardhat 网络。
- console.log 调用可以在其他网络中运行，如主网、kovan、ropsten 等。在这些网络中，它们什么都不做，但会花费极少的 gas。


### `console.log(format[,...args])`
`console.log()` 方法使用第一个参数作为类似 printf 的格式字符串，该字符串可以包含零个或多个格式说明符。每个说明符被相应的参数转换后的值替换。支持的说明符有：

- `%s`：字符串将用于将所有值转换为人类可读的字符串。`uint256`、`int256` 和 `bytes` 值被转换为其 `0x` 十六进制编码值。
- `%d`：数字将用于将所有值转换为人类可读的字符串。这与 `%s` 相同。
- `%i`：与 `%d` 工作方式相同。
- `%e`：数字的指数表示。适用于 `uint256` 和 `int256` 类型。
- `%x`：数字的十六进制表示。适用于 `uint256` 和 `int256` 类型。
- `%o`：对象。具有通用 JavaScript 风格对象格式的对象字符串表示。对于 solidity 类型，这基本上将值的字符串表示用单引号括起来。
- `%%`：单个百分号（'%'）。这不消耗参数。
- 返回：`<string>` 格式化后的字符串

如果说明符没有对应的参数，则不会被替换：
```solidity
console.log("%s:%s", "foo");
// 返回："foo:%s"
```

不属于格式字符串的值使用其人类可读的字符串表示进行格式化。

如果传递给 console.log() 方法的参数数量多于说明符数量，额外的参数将按空格分隔连接到返回的字符串中：
```solidity
console.log("%s:%s", "foo", "bar", "baz");
// 返回："foo:bar baz"
```

如果只传递一个参数给 console.log()，则它将按原样返回，不进行任何格式化：
```solidity
console.log("%% %s");
// 返回："%% %s"
```

字符串格式说明符（`%s`）在大多数情况下应使用，除非需要其他格式说明符的特定功能。
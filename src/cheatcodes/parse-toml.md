## `parseToml`

### 签名

```solidity
// 返回与'key'对应的值
vm.parseToml(string memory toml, string memory key)
// 返回整个TOML文件
vm.parseToml(string memory toml);
```

### 描述

这些作弊码用于解析字符串形式的TOML文件，并将其转换为JSON格式。通常，它与返回整个文件字符串的`vm.readFile()`结合使用。

你可以使用`forge-std`中的`stdToml`作为辅助库，以获得更好的用户体验。

作弊码接受一个`key`来搜索TOML中的特定值，或者不带key以返回整个TOML。它将值作为abi编码的`bytes`数组返回。这意味着你需要将其`abi.decode()`为适当的类型才能正常工作，否则它会`revert`。

### JSONpath Key

`parseToml`使用一种称为JSONpath的语法来形成任意JSON文件的任意键。相同的语法（或更确切地说是一种方言）被工具[`jq`](https://stedolan.github.io/jq/)使用。

要了解更多关于语法的信息，你可以访问我们用于实现该功能的rust库的[README](https://crates.io/crates/jsonpath-rust)。这样你可以确保使用正确的jsonPath方言。

### 编码规则

我们使用术语`string`、`integer`、`float`、`boolean`、`array`、`datetime`、`inline-table`，因为它们在[TOML规范](https://www.w3schools.io/file/toml-datatypes/)中定义。

我们使用术语`number`、`string`、`object`、`array`、`boolean`、`null`，因为它们在[JSON规范](https://www.w3schools.com/js/js_json_datatypes.asp)中定义。

**TOML编码规则**

- `float`限制为32位（即`+1.5`）。建议使用字符串以防止精度损失
- `integer`限制为64位（即`9223372036854775807`）。建议使用字符串编码大值
- 数组值不能有混合类型（即`[256, "b"]`，只能是`[256, 512]`或`["a", "b"]`）
- `datetime`在转换时编码为`string`
- `float`在转换时编码为`number`
- `integer`在转换时编码为`number`
- `inline-table`（或`table`）在转换时编码为`object`
- `null`编码为`"null"`字符串

**JSON编码规则**

- `null`编码为`bytes32(0)`或`""`
- 大于等于0的数字编码为`uint256`
- 负数编码为`int256`
- 不允许使用带小数位的浮点数
- 使用科学计数法的浮点数可以是`uint256`或`int256`，取决于值
- 可以解码为`H160`类型且以`0x`开头的字符串编码为`address`。换句话说，如果它可以解码为地址，它可能就是一个地址
- 以`0x`开头的字符串，如果长度为`66`则编码为`bytes32`，否则编码为`bytes`
- 既不是`address`、`bytes32`也不是`bytes`的字符串，编码为`string`
- 数组编码为其第一个元素类型的动态数组
- 对象（`{}`）编码为`tuple`

### 类型强制

如上所述，`parseToml`需要推断TOML值的类型，这有一些固有的限制。因此，有一组`parseToml*`作弊码强制返回值的类型。

例如`vm.parseTomlUint(toml, key)`会将值强制转换为`uint256`。这意味着它可以解析以下所有值并将其返回为`uint256`。这包括类型为`number`的数字、类型为`string`的字符串化数字及其十六进制表示。

```toml
hexUint = "0x12C980"
stringUint = "115792089237316195423570985008687907853269984665640564039457584007913129639935"
numberUint = 9223372036854775807 # TOML限制为64位整数
```

同样，所有类型（包括`bytes`和`bytes32`）及其数组（`vm.parseTomlUintArray`）都有作弊码。

### 将TOML表解码为Solidity结构体

TOML表转换为JSON对象。JSON对象编码为元组，可以通过元组或结构体解码。这意味着你可以在Solidity中定义一个`struct`，它将整个JSON对象解码为该`struct`。

例如：

以下TOML：

```toml
a = 43
b = "sigma"
```

将转换为以下JSON：

```json
{
  "a": 43,
  "b": "sigma"
}
```

将解码为：

```solidity
struct Json {
    uint256 a;
    string b;
}
```

由于值作为abi编码的元组返回，结构体的属性名称不需要与JSON中的键名称匹配。上述json文件也可以解码为：

```solidity
struct Json {
    uint256 apple;
    string pineapple;
}
```

重要的是字母顺序。由于JSON对象是无序数据结构，而元组是有序的，我们必须以某种方式为JSON赋予顺序。最简单的方法是按字母顺序排序键。这意味着为了正确解码JSON对象，你需要定义结构体的属性**类型**，这些类型对应于JSON键的字母顺序的值。

- 结构体按顺序解释。这意味着元组的第一个项目将根据结构体定义的第一个项目解码（不按字母顺序）。
- JSON将按字母顺序解析，不按顺序。
- 注意，这种解析使用Rust的BTreeMap crate，这意味着大写和小写字符串的处理方式不同。大写字符*优先*于小写字符，即"Zebra"会优先于"apple"。

因此，JSON的第一个（按字母顺序）值将进行abi编码，然后尝试根据`struct`的第一个属性的类型进行abi解码。

上述TOML不能用以下结构体解码：

```solidity
struct Json {
    uint256 b;
    uint256 a;
}
```

原因是它将尝试将字符串`"sigma"`解码为uint。确切地说，它会被解码，但会得到一个错误的数字，因为它会错误地解释字节。

另一个例子，给定以下TOML：

```toml
name = "Fresh Fruit"

[[apples]]
sweetness = 7
sourness = 3
color = "Red"

[[apples]]
sweetness = 5
sourness = 5
color = "Green"

[[apples]]
sweetness = 9
sourness = 1
color = "Yellow"
```

将转换为以下JSON：

```json
{
    "apples": [
        {
            "sweetness": 7,
            "sourness": 3,
            "color": "Red"
        },
        {
            "sweetness": 5,
            "sourness": 5,
            "color": "Green"
        },
        {
            "sweetness": 9,
            "sourness": 1,
            "color": "Yellow"
        }
    ],
    "name": "Fresh Fruit"
}
```

并定义如下Solidity结构体：

```solidity
struct Apple {
    string color;
    uint8 sourness;
    uint8 sweetness;
}

struct FruitStall {
    Apple[] apples;
    string name;
}
```

可以如下解码TOML：

```solidity
string memory root = vm.projectRoot();
string memory path = string.concat(root, "/src/test/fixtures/fruitstall.toml");
string memory toml = vm.readFile(path);
bytes memory data = vm.parseToml(toml);
FruitStall memory fruitstall = abi.decode(data, (FruitStall));

// 日志: Welcome to Fresh Fruit
console2.log("Welcome to", fruitstall.name);

for (uint256 i = 0; i < fruitstall.apples.length; i++) {
    Apple memory apple = fruitstall.apples[i];

    // 日志:
    // Color: Red, Sourness: 3, Sweetness: 7
    // Color: Green, Sourness: 5, Sweetness: 5
    // Color: Yellow, Sourness: 1, Sweetness: 9
    console2.log(
        "Color: %s, Sourness: %d, Sweetness: %d",
        apple.color,
        apple.sourness,
        apple.sweetness
    );
}
```

### 如何使用StdToml

1. 导入库`import "../StdToml.sol";`
2. 使用`string`定义其用法：`using stdToml for string;`
3. 如果你想解析简单值（数字、地址等），使用辅助函数
4. 如果你想解析整个TOML表：
   1. 在Solidity中定义`struct`。确保遵循字母顺序——调试起来很困难
   2. 使用`parseRaw()`辅助函数返回abi编码的`bytes`，然后将其解码为你的结构体

```solidity
string memory root = vm.projectRoot();
string memory path = string.concat(root, "/src/test/fixtures/config.toml");
string memory toml = vm.readFile(path);
bytes memory data = toml.parseRaw(".");
Config memory config = abi.decode(data, (Config))
```

### 故障排除

#### 无法读取文件

> FAIL. 原因: 路径`<file-path>`不允许进行读取操作

如果你收到此错误，请确保在`foundry.toml`中使用[`fs_permissions`键](./fs.md)启用读取权限

### 参考

- 辅助库: [stdToml.sol](https://github.com/foundry-rs/forge-std/blob/master/src/StdToml.sol)
- [文件作弊码](./fs.md): 用于处理文件的作弊码
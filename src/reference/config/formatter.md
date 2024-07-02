## 格式化工具

与 Forge 格式化工具行为相关的配置。这些键位于 `[fmt]` 部分下。

### `single_line_statement_blocks`

> 这允许独立于行长度配置语句块的样式。

Foundry 中的 `single_line_statement_blocks` 配置选项控制语句块（用 `{ }` 包围的代码块）是否会被格式化为单行或多行。

它有三个可能的值：

-   `"single"` - 如果可能，语句块将被格式化为单行。
-   `"multi"` - 语句块将始终被格式化为多行。
-   `"preserve"` - 保持语句块现有的单行/多行格式。

例如，使用 `"single"`：

```solidity
if (true) { return true; }
```

使用 `"multi"`：

```solidity
if (true) {
  return true;
}
```

使用 `"preserve"`，将保持代码的原始格式。

### `multiline_func_header`

> 这允许配置长函数头的格式。

- 类型: 字符串
- 默认值: `attributes_first`
- 环境变量: `FOUNDRY_FMT_MULTILINE_FUNC_HEADER` 或 `DAPP_FMT_MULTILINE_FUNC_HEADER`

Foundry 中的 `multiline_func_header` 配置选项控制当函数头（包含函数名、参数、返回值等的行）超过最大行长度时的格式。

多行函数头的样式，如果它不适合一行。有效值：

-   `"attributes_first"` - （默认）首先将函数属性多行写入
-   `"params_first"` - 将函数头分成多行，每个参数单独一行。函数名保持在第一行。
-   `"all"` - 如果函数参数或属性是多行的，则将所有内容多行

例如，使用 `"params_first"`：

```solidity
function myFunction(
    uint256 param1, 
    uint256 param2,
    uint256 param3
) public returns (uint256) {
  // ...
}
```

使用 `"all"`：

```solidity
function myFunction(
    uint256 param1,
    uint256 param2, 
    uint256 param3
) 
    public 
    returns (uint256) 
{
  // ...  
}
```

### `sort_imports`

> 帮助组织导入并使其更容易快速找到特定的导入。

Foundry 中的 `sort_imports` 配置选项控制是否按字母顺序对导入语句进行排序。

从提供的代码片段中：

-   在 `fmt.rs` 中，`sort_imports` 是一个可以设置为 `true` 或 `false` 的配置选项。
-   在 `formatter.rs` 中，调用 `sort_imports()` 方法以在启用 `sort_imports` 时按字母顺序对导入语句进行排序。
-   它找到源代码中由空行分隔的导入组。
-   然后按字母顺序对每个组内的导入语句进行排序。

因此，启用 `sort_imports` 将按字母顺序重新排列 solidity 导入语句，同时保留组之间的相对顺序。

### `contract_new_lines`

> 控制合约定义前后空白行的添加。

Foundry 中的 `contract_new_lines` 配置选项控制是否在合约定义前后添加空行。

启用时（`contract_new_lines = true`）：

-   在合约定义开始前添加一个空行。
-   在合约定义结束后也添加一个空行。

例如，启用 `contract_new_lines`：

```solidity
// 合约前的空行
contract MyContract {
  // ...
}
// 合约后的空行
```

这有助于视觉上分隔合约定义并提高可读性。

禁用时（`contract_new_lines = false`），合约前后不会添加空行。

因此，`contract_new_lines` 选项控制合约定义前后空白行的添加。

### `override_spacing`

Foundry 中的 `override_spacing` 配置选项控制是否在 `override` 关键字和父合约之间打印空格。

启用时：

-   在 `override` 关键字后添加一个空格。

例如：

```solidity
contract Child is Parent {
  function foo() override (Parent) public { }
}
```

禁用时，`override` 和父合约之间没有空格：

```solidity
contract Child is Parent {
  function foo() override(Parent) public { } 
}
```

启用配置 `override` 声明的样式。

-   `override_spacing = true` - 在 `override` 后打印一个空格
-   `override_spacing = false` - 不在 `override` 后打印空格

### `line_length`

- 类型: 数字
- 默认值: 120
- 环境变量: `FOUNDRY_FMT_LINE_LENGTH` 或 `DAPP_FMT_LINE_LENGTH`

格式化工具尝试换行的最大行长度。

### `tab_width`

- 类型: 数字
- 默认值: 4
- 环境变量: `FOUNDRY_FMT_TAB_WIDTH` 或 `DAPP_FMT_TAB_WIDTH`

每个缩进级别的空格数。

### `bracket_spacing`

- 类型: 布尔值
- 默认值: false
- 环境变量: `FOUNDRY_FMT_BRACKET_SPACING` 或 `DAPP_FMT_BRACKET_SPACING`

是否在括号之间打印空格。

### `int_types`

- 类型: 字符串
- 默认值: `long`
- 环境变量: `FOUNDRY_FMT_INT_TYPES` 或 `DAPP_FMT_INT_TYPES`

uint/int256 类型的样式。有效值：

- `long`（默认）: 使用显式的 `uint256` 或 `int256`
- `short`: 使用隐式的 `uint` 或 `int`
- `preserve`: 使用源代码中定义的类型

### `quote_style`

- 类型: 字符串
- 默认值: `double`
- 环境变量: `FOUNDRY_FMT_QUOTE_STYLE` 或 `DAPP_FMT_QUOTE_STYLE`

定义引号样式。有效值：

- `double`（默认）: 尽可能使用双引号 (`"`)
- `single`: 尽可能使用单引号 (`'`)
- `preserve`: 使用源代码中定义的引号

### `number_underscore`

- 类型: 字符串
- 默认值: `preserve`
- 环境变量: `FOUNDRY_FMT_NUMBER_UNDERSCORE` 或 `DAPP_FMT_NUMBER_UNDERSCORE`

数字字面量中下划线的样式。有效值：

- `preserve`（默认）: 使用源代码中定义的下划线
- `thousands`: 如果大于 9999，每千位添加一个下划线。例如 `1000` 格式化为 `1000`，`10000` 格式化为 `10_000`
- `remove`: 移除所有下划线

### `wrap_comments`

- 类型: 布尔值

- 默认值: false
- 环境变量: `FOUNDRY_FMT_WRAP_COMMENTS` 或 `DAPP_FMT_WRAP_COMMENTS`

是否在达到 `line_length` 时换行注释。

### `ignore`

- 类型: 字符串数组（模式）
- 默认值: `[]`
- 环境变量: `FOUNDRY_FMT_IGNORE` 或 `DAPP_FMT_IGNORE`

格式化时要忽略的文件列表。这是一个逗号分隔的 glob 模式列表。
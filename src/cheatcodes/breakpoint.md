## `breakpoint`

### 签名

```solidity
function breakpoint(string) external;
function breakpoint(string, bool) external;
```

### 描述

在调试器视图中放置一个断点以跳转。

调用 `vm.breakpoint('<char>, true)` 等同于 `vm.breakpoint('<char>)`，但调用 `vm.breakpoint('<char>, false)` 将删除 `'<char>` 处的断点。

如果字符被覆盖，只有最后访问的那个断点会被考虑。

### 示例

```solidity
function testBreakpoint() public {
    vm.breakpoint("a");
}
```

在测试环境中打开调试器并按下 `'a` 后，调试器步骤将放置在调用断点作弊码的地方。

![breakpoint a](../images/breakpoint.png)

### 另请参阅

[debugger](../forge/debugger.md)
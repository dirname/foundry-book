## 内联测试配置
Foundry 用户可以使用环境变量和 `foundry.toml` 中的配置语句来指定总体测试配置。详细描述请查看 [`测试参考`][Testing Reference]。

尽管这在一般情况下可能有效，但某些测试可能需要更精细的配置控制。为此，Forge 提供了一种方法，可以在不变测试和模糊测试场景中指定每项测试的配置。

用户可以直接在 Solidity 注释中内联测试配置语句。这将影响 `forge test` 命令在特定测试实例中的行为，如下例所示。

```solidity
contract MyTest is Test {
  /// forge-config: default.fuzz.runs = 100
  /// forge-config: ci.fuzz.runs = 500
  function test_SimpleFuzzTest(uint256 x) public {
    // --- snip ---
  }
}
```

这里我们要求模糊测试器分别在 `default` 和 `ci` 配置文件中运行 `100` 次和 `500` 次。有趣的是，这将覆盖任何全局级别的模糊测试 `runs` 设置。所有其他配置将从全局上下文中继承，使其成为所有可能配置的回退。

### 块注释
内联测试配置也可以在块注释中表示，如下例所示。

```solidity
contract MyTest is Test {
  /**
   * forge-config: default.fuzz.runs = 1024
   * forge-config: default.fuzz.max-test-rejects = 500
   */
  function test_SimpleFuzzTest(uint256 x) public {
    // --- snip ---
  }
}
```

### 内联模糊测试配置
用户可以指定表格中描述的配置。每个语句必须具有 `forge-config: ${PROFILE}.fuzz.` 形式的前缀。

| 参数 | 类型 | 描述 |
|-|-|-|
|`runs`|整数|为此特定测试用例执行的模糊测试次数。[`参考`][testing]。|
|`max-test-rejects`|整数|在测试整体中止之前可能被拒绝的组合输入的最大数量。[`参考`][Max test rejects]。|

模糊测试配置示例
```solidity
contract MyFuzzTest is Test {
  /// forge-config: default.fuzz.runs = 100
  /// forge-config: default.fuzz.max-test-rejects = 2
  function test_InlineConfig(uint256 x) public {
    // --- snip ---
  }
}
```

### 内联不变测试配置
用户可以指定表格中描述的配置。每个语句必须具有 `forge-config: ${PROFILE}.invariant.` 形式的前缀。

| 参数 | 类型 | 描述 |
|-|-|-|
|`runs`|整数|为此特定测试用例执行的不变测试次数。[`参考`][Invariant runs]。
|`depth`|整数|在一次运行中尝试破坏不变性的调用次数。[`参考`][Invariant depth]。
|`fail-on-revert`|布尔值|如果在不变模糊测试中发生回滚，则失败。[`参考`][Fail on revert]。
|`call-override`|布尔值|在运行不变测试时覆盖不安全的对外调用。[`参考`][Invariant call override]。

不变测试配置示例
```solidity
contract MyInvariantTest is Test {
  /// forge-config: default.invariant.runs = 100
  /// forge-config: default.invariant.depth = 2
  /// forge-config: default.invariant.fail-on-revert = false
  /// forge-config: default.invariant.call-override = true
  function invariant_InlineConfig() public {
    // --- snip ---
  }
}
```
## 模糊测试

Forge 支持基于属性的测试。

基于属性的测试是一种测试通用行为而非孤立场景的方法。

让我们通过编写一个单元测试，找出我们正在测试的通用属性，并将其转换为基于属性的测试来理解这意味着什么：

```solidity
{{#include ../../projects/fuzz_testing/test/Safe.t.sol.1:all}}
```

运行测试，我们看到它通过了：

```sh
{{#include ../output/fuzz_testing/forge-test-no-fuzz:all}}
```

这个单元测试确实测试了我们可以从安全合约中提取以太币。然而，谁又能说它对所有金额都有效，而不仅仅是 1 以太币呢？

这里的通用属性是：给定一个安全余额，当我们提取时，我们应该得到安全合约中的所有以太币。

Forge 会将任何至少有一个参数的测试视为基于属性的测试，所以让我们重写：

```solidity
{{#include ../../projects/fuzz_testing/test/Safe.t.sol.2:contract_prelude}}
    // ...

{{#include ../../projects/fuzz_testing/test/Safe.t.sol.2:test}}
}
```

如果我们现在运行测试，我们可以看到 Forge 运行了基于属性的测试，但对于高值的 `amount` 它会失败：

```sh
$ forge test
{{#include ../output/fuzz_testing/forge-test-fail-fuzz:output}}
```

默认情况下，测试合约被赋予的以太币数量是 `2**96 wei`（类似于 DappTools），所以我们必须将 `amount` 的类型限制为 `uint96`，以确保我们不会尝试发送超过我们拥有的数量：

```solidity
{{#include ../../projects/fuzz_testing/test/Safe.t.sol.3:signature}}
```

现在它通过了：

```sh
{{#include ../output/fuzz_testing/forge-test-success-fuzz:all}}
```

你可能希望使用 [`assume`](../cheatcodes/assume.md) 作弊码排除某些情况。在这些情况下，模糊器将丢弃输入并开始新的模糊运行：

```solidity
function testFuzz_Withdraw(uint96 amount) public {
    vm.assume(amount > 0.1 ether);
    // 省略
}
```

有不同的方法来运行基于属性的测试，特别是参数化测试和模糊测试。Forge 仅支持模糊测试。

### 解释结果

你可能注意到模糊测试与单元测试的总结方式略有不同：

- "runs" 指的是模糊器测试的场景数量。默认情况下，模糊器将生成 256 个场景，但用户可以设置此参数和其他测试执行参数。模糊器配置详情请参见 [`这里`](#configuring-fuzz-test-execution)。
- "μ"（希腊字母 mu）是所有模糊运行中使用的平均气体量
- "~"（波浪号）是所有模糊运行中使用的中位气体量

### 配置模糊测试执行

模糊测试执行由用户可以通过 Forge 配置原语控制的参数管理。配置可以全局应用或在每个测试基础上应用。有关此主题的详细信息，请参见
 📚 [`全局配置`](../reference/config/testing.md) 和 📚 [`内联配置`](../reference/config/inline-test-config.md)。

#### 模糊测试固定装置

当你希望确保一组特定的值用作模糊参数的输入时，可以定义模糊测试固定装置。
这些固定装置可以在测试中声明为：

- 以 `fixture` 为前缀的存储数组，后跟要模糊的参数名称。例如，当模糊参数 `amount` 类型为 `uint32` 时，可以定义为

```solidity
uint32[] public fixtureAmount = [1, 5, 555];
```

- 以 `fixture` 为前缀的函数，后跟要模糊的参数名称。函数应返回用于模糊的值数组（固定大小或动态）。例如，当模糊参数名为 `owner` 类型为 `address` 时，可以定义为

```solidity
function fixtureOwner() public returns (address[] memory)
```

如果提供的固定装置值类型与要模糊的命名参数类型不同，则会被拒绝并引发错误。

一个可以使用固定装置的例子是重现 `DSChief` 漏洞。考虑以下两个函数

```solidity
    function etch(address yay) public returns (bytes32 slate) {
        bytes32 hash = keccak256(abi.encodePacked(yay));

        slates[hash] = yay;

        return hash;
    }

    function voteSlate(bytes32 slate) public {
        uint weight = deposits[msg.sender];
        subWeight(weight, votes[msg.sender]);
        votes[msg.sender] = slate;
        addWeight(weight, votes[msg.sender]);
    }
```

漏洞可以通过在 `etch` 之前调用 `voteSlate`，并将 `slate` 值作为 `yay` 地址的哈希来重现。为了确保模糊器在同一运行中包含从 `yay` 地址派生的 `slate` 值，可以定义以下固定装置：

```solidity
    address[] public fixtureYay = [
        makeAddr("yay1"),
        makeAddr("yay2"),
        makeAddr("yay3")
    ];

    bytes32[] public fixtureSlate = [
        keccak256(abi.encodePacked(makeAddr("yay1"))),
        keccak256(abi.encodePacked(makeAddr("yay2"))),
        keccak256(abi.encodePacked(makeAddr("yay3")))
    ];
```

以下图像显示了模糊器在声明和不声明固定装置时生成值的情况：
![Fuzzer](../images/fuzzer.png)
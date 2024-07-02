## 差异测试

Forge 可以用于差异测试和差异模糊测试。你甚至可以使用 `ffi` [作弊码](../cheatcodes/ffi.md) 来测试非 EVM 可执行文件。

### 背景

[差异测试](https://en.wikipedia.org/wiki/Differential_testing) 通过比较同一函数的多个实现的输出来交叉引用这些实现。想象一下，我们有一个函数规范 `F(X)`，以及该规范的两个实现：`f1(X)` 和 `f2(X)`。我们期望对于所有存在于适当输入空间中的 x，`f1(x) == f2(x)`。如果 `f1(x) != f2(x)`，我们就知道至少有一个函数没有正确实现 `F(X)`。这种测试相等性和识别差异的过程是差异测试的核心。

差异模糊测试是差异测试的扩展。差异模糊测试通过程序生成许多 `x` 值来发现手动选择的输入可能不会揭示的差异和边缘情况。

> 注意：在这种情况下，`==` 操作符可以是相等性的自定义定义。例如，如果测试浮点实现，你可能会使用具有一定容差的近似相等性。

这种测试的一些实际应用包括：

- 比较升级后的实现与其前身
- 测试代码与已知的参考实现
- 确认与第三方工具和依赖项的兼容性

以下是一些如何使用 Forge 进行差异测试的示例。

### 入门：`ffi` 作弊码

[`ffi`](../cheatcodes/ffi.md) 允许你执行任意 shell 命令并捕获输出。以下是一个模拟示例：

```solidity
import "forge-std/Test.sol";

contract TestContract is Test {

    function testMyFFI () public {
        string[] memory cmds = new string[](2);
        cmds[0] = "cat";
        cmds[1] = "address.txt"; // 假设包含 abi 编码的地址。
        bytes memory result = vm.ffi(cmds);
        address loadedAddress = abi.decode(result, (address));
        // 对地址进行某些操作
        // ...
    }
}
```

一个地址之前已被写入 `address.txt`，我们使用 FFI 作弊码读取它。这些数据现在可以在你的测试合约中使用。

### 示例：差异测试 Merkle Tree 实现

[Merkle Trees](https://en.wikipedia.org/wiki/Merkle_tree) 是一种在区块链应用中常用的加密承诺方案。它们的流行导致了许多不同的 Merkle Tree 生成器、证明器和验证器的实现。Merkle 根和证明通常使用 JavaScript 或 Python 等语言生成，而证明验证通常在链上的 Solidity 中进行。

[Murky](https://github.com/dmfxyz/murky) 是一个完整的 Merkle 根、证明和验证的 Solidity 实现。它的测试套件包括与 OpenZeppelin 的 Merkle 证明库的差异测试，以及与参考 JavaScript 实现的根生成测试。这些测试由 Foundry 的模糊测试和 `ffi` 功能驱动。

#### 与参考 TypeScript 实现的差异模糊测试

使用 `ffi` 作弊码，Murky 测试其自己的 Merkle 根实现与 TypeScript 实现的差异，使用 Forge 模糊器提供的数据：

```solidity
function testMerkleRootMatchesJSImplementationFuzzed(bytes32[] memory leaves) public {
    vm.assume(leaves.length > 1);
    bytes memory packed = abi.encodePacked(leaves);
    string[] memory runJsInputs = new string[](8);

    // 构建 ffi 命令字符串
    runJsInputs[0] = 'npm';
    runJsInputs[1] = '--prefix';
    runJsInputs[2] = 'differential_testing/scripts/';
    runJsInputs[3] = '--silent';
    runJsInputs[4] = 'run';
    runJsInputs[5] = 'generate-root-cli';
    runJsInputs[6] = leaves.length.toString();
    runJsInputs[7] = packed.toHexString();

    // 运行命令并捕获输出
    bytes memory jsResult = vm.ffi(runJsInputs);
    bytes32 jsGeneratedRoot = abi.decode(jsResult, (bytes32));

    // 使用 Murky 计算根
    bytes32 murkyGeneratedRoot = m.getRoot(leaves);
    assertEq(murkyGeneratedRoot, jsGeneratedRoot);
}
```

> 注意：请参见 Murky Repo 中的 [`Strings2.sol`](https://github.com/dmfxyz/murky/blob/main/differential_testing/test/utils/Strings2.sol) 库，该库启用了 `(bytes memory).toHexString()`

Forge 运行 `npm --prefix differential_testing/scripts/ --silent run generate-root-cli {numLeaves} {hexEncodedLeaves}`。这使用参考 JavaScript 实现计算输入数据的 Merkle 根。脚本将根打印到 stdout，并且该打印输出作为 `bytes` 在 `vm.ffi()` 的返回值中捕获。

然后，测试使用 Solidity 实现计算根。

最后，测试断言两个根完全相等。如果不相等，测试将失败。

#### 与 OpenZeppelin 的 Merkle 证明库的差异模糊测试

你可能希望对另一个 Solidity 实现进行差异测试。在这种情况下，不需要 `ffi`。相反，参考实现直接导入到测试中。

```solidity
import "openzeppelin-contracts/contracts/utils/cryptography/MerkleProof.sol";
//...
function testCompatibilityOpenZeppelinProver(bytes32[] memory _data, uint256 node) public {
    vm.assume(_data.length > 1);
    vm.assume(node < _data.length);
    bytes32 root = m.getRoot(_data);
    bytes32[] memory proof = m.getProof(_data, node);
    bytes32 valueToProve = _data[node];
    bool murkyVerified = m.verifyProof(root, proof, valueToProve);
    bool ozVerified = MerkleProof.verify(proof, root, valueToProve);
    assertTrue(murkyVerified == ozVerified);
}
```

#### 与已知边缘情况的差异测试

差异测试并不总是模糊的——它们对于测试已知的边缘情况也很有用。在 Murky 代码库的情况下，`log2ceil` 函数的初始实现对于某些长度接近 2 的幂的数组（如 129）不起作用。作为一种安全检查，总是对这种长度的数组运行测试，并与 TypeScript 实现进行比较。你可以看到完整的测试 [这里](https://github.com/dmfxyz/murky/blob/main/differential_testing/test/DifferentialTests.t.sol#L21)。

#### 与参考数据的标准化测试

FFI 对于将可重复的标准化数据注入测试环境也很有用。在 Murky 库中，这被用作气体快照的基准（参见 [forge snapshot](./gas-snapshots.md)）。

```solidity
bytes32[100] data;
uint256[8] leaves = [4, 8, 15, 16, 23, 42, 69, 88];

function setUp() public {
    string[] memory inputs = new string[](2);
    inputs[0] = "cat";
    inputs[1] = "src/test/standard_data/StandardInput.txt";
    bytes memory result =  vm.ffi(inputs);
    data = abi.decode(result, (bytes32[100]));
    m = new Merkle();
}

function testMerkleGenerateProofStandard() public view {
    bytes32[] memory _data = _getData();
    for (uint i = 0; i < leaves.length; ++i) {
        m.getProof(_data, leaves[i]);
    }
}
```

`src/test/standard_data/StandardInput.txt` 是一个包含编码的 `bytes32[100]` 数组的文本文件。它在测试之外生成，可以在任何语言的 Web3 SDK 中使用。它看起来像：

```ignore
0xf910ccaa307836354233316666386231414464306335333243453944383735313..423532
```

标准化测试合约使用 `ffi` 读取文件。它将数据解码为数组，然后在这个示例中，为 8 个不同的叶子生成证明。由于数据是常量和标准的，我们可以使用此测试有意义地测量气体和性能改进。

> 当然，可以直接将数组硬编码到测试中！但这使得跨合约、实现等的一致测试变得更加困难。

### 示例：差异测试渐进式荷兰拍卖

Paradigm 的 [渐进式荷兰拍卖](https://www.paradigm.xyz/2022/04/gda) 机制的参考实现包含了许多差异的、模糊的测试。这是一个很好的仓库，可以进一步探索使用 `ffi` 进行差异测试。

- 对 [离散 GDA](https://github.com/FrankieIsLost/gradual-dutch-auction/blob/master/src/test/DiscreteGDA.t.sol#L78) 的差异测试
- 对 [连续 GDA](https://github.com/FrankieIsLost/gradual-dutch-auction/blob/master/src/test/ContinuousGDA.t.sol#L89) 的差异测试
- 参考 [Python 实现](https://github.com/FrankieIsLost/gradual-dutch-auction/blob/master/analysis/compute_price.py)

### 参考仓库

- [渐进式荷兰拍卖](https://github.com/FrankieIsLost/gradual-dutch-auction)
- [Murky](https://www.github.com/dmfxyz/murky)
- [Solidity 模糊测试模板](https://github.com/patrickd-/solidity-fuzzing-boilerplate)

如果你有另一个可以作为参考的仓库，请贡献它！
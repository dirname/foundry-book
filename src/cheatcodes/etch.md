## `etch`

### 签名

```solidity
function etch(address who, bytes calldata code) external;
```

### 描述

将地址 `who` 的字节码设置为 `code`。

### 示例

```solidity
bytes memory code = address(awesomeContract).code;
address targetAddr = makeAddr("target");
vm.etch(targetAddr, code);
log_bytes(address(targetAddr).code); // 0x6080604052348015610010...
```

#### 使用 `vm.etch` 启用自定义预编译

某些链，如 Blast 或 Arbitrum，运行时带有自定义预编译。Foundry 在 vanilla EVM 上操作，并不知道这些预编译。如果你遇到由于预编译不可用而导致的回滚，可以使用 `vm.etch` 作弊码将缺失预编译的模拟注入到预期出现的地址。

```solidity
{{#include ../../projects/cheatcodes/test/BlastMock.t.sol:all}}
```

<div class="warning">

注入预编译的模拟可能很棘手，因为这样的模拟不会完全模拟链上实际预编译的行为。

在上面的例子中，模拟不会导致实际收益的累积，如果配置了任何收益模式。

</div>


### 另请参阅

Forge 标准库

- [`deployCode`](../reference/forge-std/deployCode.md)
- [`deployCodeTo`](../reference/forge-std/deployCodeTo.md)
## Std Storage

Std Storage 是一个使操作存储变得简单的库。

要在测试合约中使用 Std Storage，请导入以下内容：

```solidity
import {stdStorage, StdStorage} from "forge-std/Test.sol";              
```

在测试合约中添加以下行：

```solidity
using stdStorage for StdStorage;
```

然后，通过 `stdstore` 实例访问 Std Storage。

### 函数

查询函数：

- [`target`](./target.md): 设置目标合约的地址
- [`sig`](./sig.md): 设置要静态调用的函数的 4 字节选择器
- [`with_key`](./with_key.md): 传递函数的参数（可以多次使用）
- [`depth`](./depth.md): 设置值在 `tuple` 中的位置（例如在 `struct` 内部）

终止函数：

- [`find`](./find.md): 返回槽号
- [`checked_write`](./checked_write.md): 设置要写入存储槽的数据
- [`read_<type>`](./read.md): 以 `<type>` 类型从存储槽读取值

### 示例

`playerToCharacter` 跟踪玩家角色的信息。

```solidity
// MetaRPG.sol

struct Character {
    string name;
    uint256 level;
}

mapping (address => Character) public playerToCharacter;
```

假设我们要将角色的等级设置为 120。

```solidity
// MetaRPG.t.sol

stdstore
    .target(address(metaRpg))
    .sig("playerToCharacter(address)")
    .with_key(address(this))
    .depth(1)
    .checked_write(120);
```

### 限制

- 不支持访问打包槽

### 已知问题

- 如果 `tuple` 包含短于 32 字节的类型，槽可能无法找到
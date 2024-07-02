## `setEnv`

### 签名

```solidity
function setEnv(string calldata key, string calldata value) external;
```

### 描述

设置一个环境变量 `key=value`。

> ℹ️ **注意**
>
> 由进程设置的环境变量只能由其自身及其子进程访问。因此，调用 `setEnv` 只会修改当前运行的 `forge` 进程的环境变量，而不会影响 shell（`forge` 的父进程），即它们不会在 `forge` 进程退出后持久存在。

### 提示

- 环境变量键不能为空。
- 环境变量键不能包含等号 `=` 或 NUL 字符 `\0`。
- 环境变量值不能包含 NUL 字符 `\0`。

### 示例

```solidity
string memory key = "hello";
string memory val = "world";
cheats.setEnv(key, val);
```
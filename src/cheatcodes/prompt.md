## `prompt`

### 签名

```solidity
function prompt(string calldata promptText) external returns (string memory input);
function promptSecret(string calldata promptText) external returns (string memory input);
function promptSecretUint(string calldata promptText) external returns (uint256);
```

### 描述

向用户显示一个交互式提示，用于插入任意数据。

`vm.prompt` 显示一个交互式输入，而 `vm.promptSecret` 和 `vm.promptSecretUint` 显示一个隐藏输入，用于密码和其他不应泄露到终端的秘密信息。

> ℹ️ **注意**
>
> 这个作弊码旨在用于脚本——不是测试。建议遵循以下最佳实践来测试使用 `vm.prompt` 的脚本并处理超时，因为脚本可能会挂起或回滚。在非交互式 shell 中运行时，此作弊码会回滚。

### 配置

为了防止不必要的挂起，`vm.prompt` 有一个超时配置。

在你的 `foundry.toml` 文件中：

```toml
prompt_timeout = 120
```

默认值是 `120`，单位是秒。

### 最佳实践

#### 测试使用 `vm.prompt` 的脚本

当测试包含 `vm.prompt` 的脚本时，建议使用以下模式：

```solidity
contract Script {
    function run() public {
        uint256 myUint = vm.parseUint(vm.prompt("输入一个整数"));
        run(myUint);
    }

    function run(uint256 myUint) public {
        // 实际逻辑
    }
}
```

这样，我们保持了用户体验的增益（运行脚本时不需要提供 `--sig` 参数），但测试可以设置任何值给 `myUint`，而不仅仅是一个硬编码的默认值。

#### 处理超时

当用户在超时到期之前未能提供输入时，`vm.prompt` 作弊码会回滚。如果你愿意，可以使用 `try/catch` 处理超时：

```solidity
string memory input;

try vm.prompt("用户名") returns (string memory res) {
    input = res;
}
catch (bytes memory) {
    input = "匿名";
}
```

### 示例

#### 选择 RPC 端点

提供一个选项来选择要运行的 RPC/链。

在你的 `foundry.toml` 文件中：

```toml
[rpc_endpoints]
mainnet = "https://eth.llamarpc.com"
polygon = "https://polygon.llamarpc.com"
```

在你的脚本中：

```solidity
string memory rpcEndpoint = vm.prompt("RPC 端点");
vm.createSelectFork(rpcEndpoint);
```

#### 将用户输入解析为本机类型

我们可以使用字符串解析作弊码来解析用户的响应：

```solidity
uint privateKey = vm.promptSecretUint("私钥");
address to = vm.parseAddress(vm.prompt("发送至"));
uint amount = vm.parseUint(vm.prompt("金额 (wei)"));
vm.broadcast(privateKey);
payable(to).transfer(amount);
```
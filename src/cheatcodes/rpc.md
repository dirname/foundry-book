## RPC 相关的作弊码

### 签名

```solidity
// 返回配置别名的 URL
function rpcUrl(string calldata alias) external returns (string memory);
// 返回所有配置的 (别名, URL) 对
function rpcUrls() external returns(string[2][] memory);
/// 使用配置的分叉 URL 执行 Ethereum JSON-RPC 请求。
function rpc(string calldata method, string calldata params) external returns (bytes memory data);
```

### 描述

提供作弊码以访问 `foundry.toml` 文件中 `rpc_endpoints` 对象配置的所有 RPC 端点，并能够使用配置的分叉 URL 进行 `rpc` 调用。

### 示例

以下 `rpc_endpoints` 在 `foundry.toml` 中注册了两个 RPC 别名：

- `optimism` 直接引用 URL
- `mainnet` 引用包含实际 URL 的 `RPC_MAINNET` 环境值

*环境变量需要用 `${}` 包裹*

```toml
# --snip--
[rpc_endpoints]
optimism = "https://optimism.alchemyapi.io/v2/..."
mainnet = "${RPC_MAINNET}" 
```

```solidity
string memory url = vm.rpcUrl("optimism");
assertEq(url, "https://optimism.alchemyapi.io/v2/...");
```

如果环境变量缺失，`rpcUrl()` 将回滚：

```solidity
vm.expectRevert("Failed to resolve env var `${RPC_MAINNET}` in `RPC_MAINNET`: environment variable not found");
string memory url = vm.rpcUrl("mainnet");
```

检索所有可用的别名 -> URL 对

```solidity
string[2][] memory allUrls = vm.rpcUrls();
assertEq(allUrls.length, 2);

string[2] memory val = allUrls[0];
assertEq(val[0], "optimism");

string[2] memory env = allUrls[1];
assertEq(env[0], "mainnet");
```

进行 `eth_getBalance` 的 RPC 调用

```solidity
// 区块 <https://etherscan.io/block/18332681> 的余额
bytes memory result = vm.rpc("eth_getBalance", "[\"0x8D97689C9818892B700e27F316cc3E41e17fBeb9\", \"0x117BC09\"]")
assertEq(hex"10b7c11bcb51e6", result);
```

### 另请参阅

Forge 配置

[配置参考](../reference/config/testing.md#rpc_endpoints)
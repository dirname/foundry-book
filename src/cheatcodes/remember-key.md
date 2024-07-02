## `rememberKey`

### 签名

```solidity
function rememberKey(uint256 privateKey) external returns (address);
```

### 描述

将私钥存储在 forge 的本地钱包中，并返回相应的地址，该地址稍后可用于[广播](./broadcast.md)。

### 示例

从测试助记词路径 `m/44'/60'/0'/0/0` 派生私钥，将其存储在 forge 的钱包中，并使用它开始广播交易：

```solidity
string memory mnemonic = "test test test test test test test test test test test junk";
uint256 privateKey = vm.deriveKey(mnemonic, 0);
address deployer = vm.rememberKey(privateKey);

vm.startBroadcast(deployer);
...
vm.stopBroadcast();
```

从 `PRIVATE_KEY` 环境变量加载私钥，并使用它开始广播交易：

```solidity
address deployer = vm.rememberKey(vm.envUint("PRIVATE_KEY"));

vm.startBroadcast(deployer);
...
vm.stopBroadcast();
```

### 参见

- [deriveKey](./derive-key.md)

Forge 标准库：
- [deriveRememberKey](../reference/forge-std/derive-remember-key.md)
## `deriveRememberKey`

### 签名

```solidity
function deriveRememberKey(string memory mnemonic, uint32 index) internal returns (address who, uint256 privateKey)
```

### 描述

从助记词派生一个私钥，并将其存储在 forge 的本地钱包中。返回地址和私钥。

### 示例

从测试助记词在路径 `m/44'/60'/0'/0/0` 中获取私钥和地址。使用它们来签名一些数据并开始广播交易：

```solidity
string memory mnemonic = "test test test test test test test test test test test junk";

(address deployer, uint256 privateKey) = deriveRememberKey(mnemonic, 0);

bytes32 hash = keccak256("Signed by deployer");
(uint8 v, bytes32 r, bytes32 s) = vm.sign(privateKey, hash);

vm.startBroadcast(deployer);
...
vm.stopBroadcast();
```

从测试助记词在路径 `m/44'/60'/0'/0/0` 中获取地址以开始广播交易：

```solidity
string memory mnemonic = "test test test test test test test test test test test junk";

(address deployer, ) = deriveRememberKey(mnemonic, 0);

vm.startBroadcast(deployer);
...
vm.stopBroadcast();
```

### 另请参阅

作弊码：
- [deriveKey](../../cheatcodes/derive-key.md)
- [rememberKey](../../cheatcodes/remember-key.md)
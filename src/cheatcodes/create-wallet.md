## `createWallet`

### 签名

```solidity
  struct Wallet {
      address addr;
      uint256 publicKeyX;
      uint256 publicKeyY;
      uint256 privateKey;
  }
```

```solidity
  function createWallet(string calldata) external returns (Wallet memory);
```

```solidity
  function createWallet(uint256) external returns (Wallet memory);
```

```solidity
  function createWallet(uint256, string calldata) external returns (Wallet memory);
```

### 描述

在给定一个参数来派生私钥时，创建一个新的 Wallet 结构体。

### 提示

[`sign()`](./sign.md) 和 [`getNonce()`](./get-nonce.md) 也都支持 Wallet 结构体的函数重载。

### 示例

#### `uint256`

```solidity
Wallet memory wallet = vm.createWallet(uint256(keccak256(bytes("1"))));

emit log_uint(wallet.privateKey); // uint256(keccak256(bytes("1")))

emit log_address(wallet.addr); // vm.addr(wallet.privateKey)

emit log_address(
    address(
        uint160(
            uint256(
                keccak256(abi.encode(wallet.publicKeyX, wallet.publicKeyY))
            )
        )
    )
); // wallet.addr

emit log_string(vm.getLabel(wallet.addr)); // ""
```

#### `string`

```solidity
Wallet memory wallet = vm.createWallet("bob's wallet");

emit log_uint(wallet.privateKey); // uint256(keccak256(bytes("bob's wallet")))

emit log_address(wallet.addr); // vm.addr(wallet.privateKey)

emit log_address(
    address(
        uint160(
            uint256(
                keccak256(abi.encode(wallet.publicKeyX, wallet.publicKeyY))
            )
        )
    )
); // wallet.addr

emit log_string(vm.getLabel(wallet.addr)); // "bob's wallet"
```

#### `uint256` 和 `string`

```solidity
Wallet memory wallet = vm.createWallet(uint256(keccak256(bytes("1"))), "bob's wallet");

emit log_uint(wallet.privateKey); // uint256(keccak256(bytes("1")))

emit log_address(wallet.addr); // vm.addr(wallet.privateKey)

emit log_address(
    address(
        uint160(
            uint256(
                keccak256(abi.encode(wallet.publicKeyX, wallet.publicKeyY))
            )
        )
    )
); // wallet.addr

emit log_string(vm.getLabel(wallet.addr)); // "bob's wallet"
```
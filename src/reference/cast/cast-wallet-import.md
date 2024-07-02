## cast wallet import

### NAME

cast-wallet-import - 将私钥导入加密密钥库

### SYNOPSIS

`cast wallet import` [*options*] _account_name_

### DESCRIPTION

将私钥导入加密密钥库。

如果没有指定 _keystore-dir_，它将保存在默认的 `~/.foundry/keystores` 中，因此可以通过 `forge script`、`cast send` 或其他需要私钥的方法中的 `--account` 选项访问。

### OPTIONS

#### Directory Options

`-k`  
`--keystore-dir`

&nbsp;&nbsp;&nbsp;&nbsp;存储加密密钥库的路径。  
&nbsp;&nbsp;&nbsp;&nbsp;默认为 `~/.foundry/keystores`。

{{#include ../common/wallet-options-raw.md}}

{{#include common-options.md}}

### EXAMPLES

1. 从私钥创建密钥库：

   ```sh
   cast wallet import BOB --interactive
   ```

2. 从助记词创建密钥库：

   ```sh
   cast wallet import ALICE --mnemonic "test test test test test test test test test test test test"
   ```

3. 从助记词和特定助记词索引创建密钥库：
   ```sh
   cast wallet import ALICE --mnemonic "test test test test test test test test test test test test" --mnemonic-index 1
   ```

### SEE ALSO

[cast](./cast.md), [cast wallet](./cast-wallet.md)
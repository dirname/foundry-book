## 使用 Cast 和 Anvil 分叉主网

### 介绍

通过结合 [Anvil][anvil] 和 [Cast][cast]，您可以分叉并测试与真实网络上的合约进行交互。本教程的目标是向您展示如何将 Dai 代币从一个持有 Dai 的用户转移到由 Anvil 创建的账户。

### 设置

让我们从分叉主网开始。

```sh
anvil --fork-url https://mainnet.infura.io/v3/$INFURA_KEY
```

您将看到创建了 10 个账户及其公钥和私钥。我们将使用 `0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266`（我们称这个用户为 Alice）。

### 转移 Dai

前往 Etherscan 并搜索 Dai 代币的持有者（[这里](https://etherscan.io/token/0x6b175474e89094c44da98b954eedeac495271d0f#balances)）。让我们选择一个随机的账户。在这个例子中，我们将使用 `0xfc2eE3bD619B7cfb2dE2C797b96DeeCbD7F68e46`。让我们将合约和账户导出为环境变量：

```sh
export ALICE=0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
export DAI=0x6b175474e89094c44da98b954eedeac495271d0f
export UNLUCKY_USER=0xfc2eE3bD619B7cfb2dE2C797b96DeeCbD7F68e46
```

我们可以使用 [`cast call`][cast-call] 检查 Alice 的余额：

```sh
$ cast call $DAI \
  "balanceOf(address)(uint256)" \
  $ALICE
0
```

同样，我们也可以使用 `cast call` 检查不幸用户的余额：

```sh
$ cast call $DAI \
  "balanceOf(address)(uint256)" \
  $UNLUCKY_USER
21840114973524208109322438
```

让我们使用 [`cast send`][cast-send] 从不幸运用户向 Alice 转移一些代币：

```sh
# 这会调用 Anvil 并让我们模拟不幸用户
$ cast rpc anvil_impersonateAccount $UNLUCKY_USER
$ cast send $DAI \
--from $UNLUCKY_USER \
  "transfer(address,uint256)(bool)" \
  $ALICE \
  300000000000000000000000 \
  --unlocked
blockHash               0xbf31c45f6935a0714bb4f709b5e3850ab0cc2f8bffe895fefb653d154e0aa062
blockNumber             15052891
...
```

让我们检查转移是否成功：

```sh
cast call $DAI \
  "balanceOf(address)(uint256)" \
  $ALICE
300000000000000000000000

$ cast call $DAI \
  "balanceOf(address)(uint256)" \
  $UNLUCKY_USER
21540114973524208109322438
```

[anvil]: ../reference/anvil/
[cast]: ../reference/cast/
[cast-call]: ../reference/cast/cast-call.md
[cast-send]: ../reference/cast/cast-send.md
## 使用 Docker 化 Foundry 项目

本教程将向您展示如何使用 Foundry 的 Docker 镜像构建、测试和部署智能合约。它改编自 [solmate nft](./solmate-nft.md) 教程中的代码。如果您还没有完成该教程，并且对 Solidity 还不熟悉，建议您先从该教程开始。或者，如果您对 Docker 和 Solidity 有一定的了解，可以使用您现有的项目并相应调整。NFT 和 Docker 部分的完整源代码可在 [这里](https://github.com/dmfxyz/foundry-docker-tutorial) 找到。

> 本教程仅用于说明目的，并按原样提供。教程未经过审计也未完全测试。本教程中的代码不应在生产环境中使用。

### 安装和设置

运行本教程所需的唯一安装是 Docker，以及可选的您选择的 IDE。
请按照 [Docker 安装说明](/getting-started/installation.html#using-with-docker) 进行操作。

为了使未来的命令更简洁，让我们重新标记镜像：
`docker tag ghcr.io/foundry-rs/foundry:latest foundry:latest`

本地安装 Foundry 不是严格要求的，但可能有助于调试。您可以使用 [foundryup](/getting-started/installation.html#using-foundryup) 进行安装。

最后，要使用本教程中的 `cast` 或 `forge create` 部分，您需要访问一个以太坊节点。如果您没有运行自己的节点（很可能），可以使用第三方节点服务。本教程中不会推荐特定的提供商。了解节点即服务的好地方是 [Ethereum 的文章](https://ethereum.org/en/developers/docs/nodes-and-clients/nodes-as-a-service/)。

**在本教程的其余部分中，假设您的以太坊节点的 RPC 端点设置如下**：`export RPC_URL=<YOUR_RPC_URL>`

### Foundry Docker 镜像的介绍

Docker 镜像主要有两种使用方式：
1. 直接作为 forge 和 cast 的接口
2. 作为构建您自己的容器化测试、构建和部署工具的基础镜像

我们将涵盖这两种方式，但首先让我们看看如何使用 Docker 与 Foundry 进行交互。这也是测试本地安装是否成功的好方法！

我们可以对 Docker 镜像运行任何 `cast` [命令](/reference/cast/)。让我们获取最新的区块信息：
```sh
$ docker run foundry "cast block --rpc-url $RPC_URL latest"
baseFeePerGas        "0xb634241e3"
difficulty           "0x2e482bdf51572b"
extraData            "0x486976656f6e20686b"
gasLimit             "0x1c9c380"
gasUsed              "0x652993"
hash                 "0x181748772da2f968bcc91940c8523bb6218a7d57669ded06648c9a9fb6839db5"
logsBloom            "0x406010046100001198c220108002b606400029444814008210820c04012804131847150080312500300051044208430002008029880029011520380060262400001c538d00440a885a02219d49624aa110000003094500022c003600a00258009610c410323580032000849a0408a81a0a060100022505202280c61880c80020e080244400440404520d210429a0000400010089410c8408162903609c920014028a94019088681018c909980701019201808040004100000080540610a9144d050020220c10a24c01c000002005400400022420140e18100400e10254926144c43a200cc008142080854088100128844003010020c344402386a8c011819408"
miner                "0x1ad91ee08f21be3de0ba2ba6918e714da6b45836"
mixHash              "0xb920857687476c1bcb21557c5f6196762a46038924c5f82dc66300347a1cfc01"
nonce                "0x1ce6929033fbba90"
number               "0xdd3309"
parentHash           "0x39c6e1aa997d18a655c6317131589fd327ae814ef84e784f5eb1ab54b9941212"
receiptsRoot         "0x4724f3b270dcc970f141e493d8dc46aeba6fffe57688210051580ac960fe0037"
sealFields           []
sha3Uncles           "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347"
size                 "0x1d6bb"
stateRoot            "0x0d4b714990132cf0f21801e2931b78454b26aad706fc6dc16b64e04f0c14737a"
timestamp            "0x6246259b"
totalDifficulty      "0x9923da68627095fd2e7"
transactions         [...]
uncles               []
```

如果我们在包含一些 Solidity [源代码](https://github.com/dmfxyz/foundry-docker-tutorial) 的目录中，我们可以将该目录挂载到 Docker 中，并按需使用 `forge`。例如：

```sh
$ docker run -v $PWD:/app foundry "forge test --root /app --watch"
{{#include ../output/nft_tutorial/forge-test:output}}
```
您可以看到我们的代码完全在容器中编译和测试。此外，由于我们传递了 `--watch` 选项，容器将在检测到更改时重新编译代码。

> 注意：Foundry Docker 镜像基于 alpine 构建，旨在尽可能精简。因此，它目前不包括 `git` 等开发资源。如果您计划在容器中管理整个开发生命周期，应在 Foundry 镜像之上构建自定义开发镜像。

### 创建“构建和测试”镜像
让我们使用 Foundry Docker 镜像作为基础来构建我们自己的 Docker 镜像。我们将使用该镜像来：
1. 构建我们的 Solidity 代码
2. 运行我们的 Solidity 测试

一个简单的 `Dockerfile` 可以实现这两个目标：
```docker
# 使用最新的 foundry 镜像
FROM ghcr.io/foundry-rs/foundry

# 将我们的源代码复制到容器中
WORKDIR /app

# 构建和测试源代码
COPY . .
RUN forge build
RUN forge test
```

您可以构建这个 Docker 镜像，并在容器中观察 forge 构建/运行测试：
```sh
$ docker build --no-cache --progress=plain .
```

现在，如果我们的一个测试失败会发生什么？随意修改 `src/test/NFT.t.sol` 以使其中一个测试失败。尝试再次构建镜像。

```sh
$ docker build --no-cache --progress=plain .
<...>
#9 0.522 Failed tests:
#9 0.522 [FAIL. Reason: Ownable: caller is not the owner] testWithdrawalFailsAsNotOwner() (gas: 193917)
#9 0.522
#9 0.522 Encountered a total of 1 failing tests, 9 tests succeeded
------
error: failed to solve: executor failed running [/bin/sh -c forge test]: exit code: 1
```

我们的镜像构建失败了，因为我们的测试失败了！这实际上是一个很好的特性，因为这意味着如果我们有一个成功构建的 Docker 镜像（因此可供使用），我们就知道镜像中的代码通过了测试。*
> *当然，您的 Docker 镜像的保管链非常重要。Docker 层哈希对于验证非常有用。在生产环境中，考虑 [签署您的 Docker 镜像](https://docs.docker.com/engine/security/trust/#:~:text=To%20sign%20a%20Docker%20Image,the%20local%20Docker%20trust%20repository)。

### 创建部署镜像

现在，我们将继续构建一个更高级的 Dockerfile。让我们添加一个入口点，允许我们使用构建（并测试！）的镜像来部署我们的代码。我们先针对 Rinkeby 测试网。

```docker
# 使用最新的 foundry 镜像
FROM ghcr.io/foundry-rs/foundry

# 将我们的源代码复制到容器中
WORKDIR /app

# 构建和测试源代码
COPY . .
RUN forge build
RUN forge test

# 设置入口点为 forge 部署命令
ENTRYPOINT ["forge", "create"]
```

让我们构建镜像，这次给它一个名称：

```sh
$ docker build --no-cache --progress=plain -t nft-deployer .
```

以下是我们如何使用 Docker 镜像进行部署：
```sh
$ docker run nft-deployer --rpc-url $RPC_URL --constructor-args "ForgeNFT" "FNFT" "https://ethereum.org" --private-key $PRIVATE_KEY ./src/NFT.sol:NFT
No files changed, compilation skipped
Deployer: 0x496e09fcb240c33b8fda3b4b74d81697c03b6b3d
Deployed to: 0x23d465eaa80ad2e5cdb1a2345e4b54edd12560d3
Transaction hash: 0xf88c68c4a03a86b0e7ecb05cae8dea36f2896cd342a6af978cab11101c6224a9
```

我们刚刚完全在 Docker 容器中构建、测试和部署了我们的合约！本教程旨在用于测试网，但您可以运行完全相同的 Docker 镜像针对主网，并确信相同的代码由相同的工具部署。

### 为什么这很有用？

Docker 是关于可移植性、可重复性和环境不变性的。这意味着您可以减少在不同环境、网络、开发人员等之间切换时对意外变化的担忧。以下是一些基本的例子，说明为什么 **我** 喜欢使用 Docker 镜像进行智能合约部署：

* 减少确保系统级依赖在部署环境之间匹配的开销（例如，您的生产运行器是否总是与您的开发运行器具有相同版本的 `forge`？）
* 增加信心，确保代码在部署前经过测试且未被更改（例如，如果在上述镜像中，您的代码在部署时重新编译，这是一个重大危险信号）。
* 减轻职责分离的痛点：拥有您主网凭证的人不需要确保他们拥有最新的编译器、代码库等。很容易确保在测试网中某人运行的 Docker 部署镜像与针对主网的镜像相同。
* 冒着听起来像 Web2 的风险，Docker 在几乎所有公共云提供商中都是一个被接受的标准。它使得与区块链交互的作业、任务等的调度变得容易。

### 故障排除
如上所述，Foundry 镜像默认不包括 `git`。这可能会导致某些命令在没有明确原因的情况下失败。例如：
```bash
$ docker run foundry "forge init --no-git /test"
Initializing /test...
Installing ds-test in "/test/lib/ds-test", (url: https://github.com/dapphub/ds-test, tag: None)
Error:
   0: No such file or directory (os error 2)

Location:
   cli/src/cmd/forge/install.rs:107
```
在这种情况下，失败仍然是由缺少 `git` 安装引起的。推荐的解决方法是基于现有的 Foundry 镜像构建，并安装任何其他所需的开发依赖项。
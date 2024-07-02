## 处理现有项目

Foundry 使得在现有项目上开发几乎没有开销。

在这个示例中，我们将使用 [PaulRBerg][paul] 的 [`foundry-template`][template]。

首先，克隆项目并在项目目录中运行 [`forge install`][install]。

```sh
$ git clone https://github.com/PaulRBerg/foundry-template
$ cd foundry-template 
$ forge install
$ bun install # 安装 Solhint、Prettier 和其他 Node.js 依赖
```

我们运行 [`forge install`][install] 来安装项目中的子模块依赖。

要构建，使用 [`forge build`][build]：

```sh
{{#include ../output/foundry-template/forge-build:all}}
```

要测试，使用 [`forge test`][test]：

```sh
{{#include ../output/foundry-template/forge-test:all}}
```

[paul]: https://github.com/PaulRBerg
[template]: https://github.com/PaulRBerg/foundry-template
[install]: ../reference/forge/forge-install.md
[build]: ../reference/forge/forge-build.md
[test]: ../reference/forge/forge-test.md
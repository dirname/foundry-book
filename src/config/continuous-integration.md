## 持续集成

### GitHub Actions

要使用 GitHub Actions 测试您的项目，以下是一个示例工作流程：

```yml
on: [push]

name: test

jobs:
  check:
    name: Foundry 项目
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive

      - name: 安装 Foundry
        uses: foundry-rs/foundry-toolchain@v1
        with:
          version: nightly

      - name: 运行测试
        run: forge test -vvv
```

### Travis CI

要使用 Travis CI 测试您的项目，以下是一个示例工作流程：

```yml
language: rust
cache:
  cargo: true
  directories:
    - $HOME/.foundry

install:
  - curl -L https://foundry.paradigm.xyz | bash
  - export PATH=$PATH:$HOME/.foundry/bin
  - foundryup -b master

script:
  - forge test -vvv
```

## GitLab CI

要使用 GitLab CI 测试您的项目，以下是一个示例工作流程：
注意：请查看 [Policy](https://docs.gitlab.com/runner/executors/docker.html#how-pull-policies-work) 以获取远程镜像

```yml
variables:
  GIT_SUBMODULE_STRATEGY: recursive

jobs:
  image: ghcr.io/foundry-rs/foundry
  script:
    - forge install
    - forge test -vvv
```
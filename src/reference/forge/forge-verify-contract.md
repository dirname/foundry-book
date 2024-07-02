## forge verify-contract

### NAME

forge-verify-contract - 在选定的验证提供商上验证智能合约。

### SYNOPSIS

``forge verify-contract`` [*options*] *address* *contract*

### DESCRIPTION

在选定的验证提供商上验证智能合约。

你必须提供：
- 合约地址
- 合约名称或合约路径（详见下文）
如果是 Etherscan 验证，你还必须提供：
- 你的 Etherscan API 密钥，可以通过参数传递或设置 `ETHERSCAN_API_KEY`

要找到确切的编译器版本，运行 `~/.svm/x.y.z/solc-x.y.z --version` 并在[这里](https://etherscan.io/solcversions)搜索版本字符串中的 8 位十六进制数字。

合约路径的格式为 `<path>:<contract>`，例如 `src/Contract.sol:Contract`。

默认情况下，智能合约以多文件方式验证。如果你想在验证前展平合约，请传递 `--flatten`。

如果传递了 `--flatten`，此命令将尝试编译展平合约的源代码。如果你不想这样做，请传递 `--force`。

你可以使用 `--constructor-args` 指定 **ABI 编码**的构造函数参数。或者，你可以使用 `--constructor-args-path` 指定包含 **空格分隔**的构造函数参数的文件。（注意，为了后者工作，配置中必须启用[缓存](../config/project.html#cache)。）

### OPTIONS

#### Verify Contract Options

{{#include ../common/verifier-options.md}}

`--skip-is-verified-check`
&nbsp;&nbsp;&nbsp;&nbsp;即使合约已经验证，也发送验证请求。

`--compiler-version` *version*  
&nbsp;&nbsp;&nbsp;&nbsp;用于构建智能合约的编译器版本。

&nbsp;&nbsp;&nbsp;&nbsp;要找到确切的编译器版本，运行 `~/.svm/x.y.z/solc-x.y.z --version`，其中 `x` 和 `y` 分别是主版本号和次版本号，然后在[这里](https://etherscan.io/solcversions)搜索版本字符串中的 8 位十六进制数字。

`--num-of-optimizations` *num*  
`--optimizer-runs` *num*      
&nbsp;&nbsp;&nbsp;&nbsp;用于构建智能合约的优化运行次数。

`--constructor-args` *args*  
&nbsp;&nbsp;&nbsp;&nbsp;ABI 编码的构造函数参数。与 `--constructor-args-path` 冲突。

`--constructor-args-path` *file*  
&nbsp;&nbsp;&nbsp;&nbsp;包含构造函数参数的文件路径。与 `--constructor-args` 冲突。

`--chain-id` *chain*  
`--chain` *chain*  
&nbsp;&nbsp;&nbsp;&nbsp;合约部署到的链的 ID 或名称。  
&nbsp;&nbsp;&nbsp;&nbsp;默认：mainnet

`--flatten`  
&nbsp;&nbsp;&nbsp;&nbsp;指示是否在验证前展平源代码的标志。

&nbsp;&nbsp;&nbsp;&nbsp;如果未提供此标志，将使用 JSON 标准输入。

`-f`  
`--force`  
&nbsp;&nbsp;&nbsp;&nbsp;在验证前不编译展平的智能合约。

{{#include ../common/retry-options.md}}

`--show-standard-json-input`  
&nbsp;&nbsp;&nbsp;&nbsp;命令输出适合保存到文件并上传到区块浏览器进行验证的 JSON。

`--watch`  
&nbsp;&nbsp;&nbsp;&nbsp;提交后等待验证结果。  
&nbsp;&nbsp;&nbsp;&nbsp;自动运行 `forge verify-check` 直到验证失败或成功。

{{#include project-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 使用 JSON 标准输入在 Etherscan 上验证合约
    ```sh
    forge verify-contract <address> SomeContract --watch
    ```

2. 在自定义 Sourcify 实例上验证合约
    ```sh
    forge verify-contract --verifier sourcify \
      --verifier-url http://localhost:5000 <address> SomeContract
    ```

3. 验证使用 solc v0.8.11+commit.d7f03943 构建的展平合约
    ```sh
    forge verify-contract --flatten --watch --compiler-version "v0.8.11+commit.d7f03943" \
      --constructor-args $(cast abi-encode "constructor(string,string,uint256,uint256)" "ForgeUSD" "FUSD" 18 1000000000000000000000) \
      <address> MyToken
    ```

4. 通过指定文件中的构造函数参数验证展平合约
    ```sh
    forge verify-contract --flatten --watch --compiler-version "v0.8.11+commit.d7f03943" \
      --constructor-args-path constructor-args.txt <address> src/Token.sol:MyToken
    ```
    其中 `constructor-args.txt` 包含以下内容：
    ```text
    ForgeUSD FUSD 18 1000000000000000000000
    ```
    
5. 在部署后立即使用 Blockscout 验证合约（确保在 Blockscout 主页浏览器 URL 末尾添加 "/api?"）
    ```sh
    forge create --rpc-url <rpc_https_endpoint> --private-key $devTestnetPrivateKey src/Contract.sol:SimpleStorage --verify --verifier blockscout --verifier-url <blockscout_homepage_explorer_url>/api? 
    ```

6. 使用 Oklink 验证合约
    ```sh
    forge verify-contract 0x8CDDE82cFB4555D6ca21B5b28F97630265DA94c4 Counter --verifier oklink --verifier-url https://www.oklink.com/api/v5/explorer/contract/verify-source-code-plugin/XLAYER  --api-key $OKLINK_API_KEY
    ```
    
7. 在部署时使用 Oklink 验证合约
    ```sh
    forge create Counter --rpc-url <rpc_https_endpoint> --verify --verifier oklink --verifier-url https://www.oklink.com/api/v5/explorer/contract/verify-source-code-plugin/XLAYER --etherscan-api-key $OKLINK_API_KEY --private-key $PRIVATE_KEY --legacy
    ```

### SEE ALSO

[forge](./forge.md), [forge create](./forge-create.md), [forge flatten](./forge-flatten.md), [forge verify-check](./forge-verify-check.md)
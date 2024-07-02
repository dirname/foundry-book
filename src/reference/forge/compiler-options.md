```markdown
#### 编译器选项

`--optimize`  
&nbsp;&nbsp;&nbsp;&nbsp;激活 Solidity 优化器。

`--optimizer-runs` *runs*  
&nbsp;&nbsp;&nbsp;&nbsp;优化器运行的次数。

`--via-ir`  
&nbsp;&nbsp;&nbsp;&nbsp;使用 Yul 中间表示编译管道。

`--revert-strings`  
&nbsp;&nbsp;&nbsp;&nbsp;如何处理 revert 和 require 原因字符串。

`--use` *solc_version*  
&nbsp;&nbsp;&nbsp;&nbsp;指定 solc 版本或本地 solc 的路径进行构建。

&nbsp;&nbsp;&nbsp;&nbsp;有效值格式为 `x.y.z`、`solc:x.y.z` 或 `path/to/solc`。

`--offline`  
&nbsp;&nbsp;&nbsp;&nbsp;不访问网络。缺失的 solc 版本不会被安装。

`--no-auto-detect`  
&nbsp;&nbsp;&nbsp;&nbsp;不自动检测 solc。

`--ignored-error-codes` *error_codes*  
&nbsp;&nbsp;&nbsp;&nbsp;通过错误代码忽略 solc 警告。参数是以逗号分隔的错误代码列表。

`--extra-output` *selector*  
&nbsp;&nbsp;&nbsp;&nbsp;在合约的构件中包含的额外输出。

&nbsp;&nbsp;&nbsp;&nbsp;示例键：`abi`、`storageLayout`、`evm.assembly`、`ewasm`、`ir`、`ir-optimized`、`metadata`。

&nbsp;&nbsp;&nbsp;&nbsp;完整描述请参见 [Solidity 文档][output-desc]。

`--extra-output-files` *selector*  
&nbsp;&nbsp;&nbsp;&nbsp;写入单独文件的额外输出。

&nbsp;&nbsp;&nbsp;&nbsp;示例键：`abi`、`storageLayout`、`evm.assembly`、`ewasm`、`ir`、`ir-optimized`、`metadata`。

&nbsp;&nbsp;&nbsp;&nbsp;完整描述请参见 [Solidity 文档][output-desc]。

`--evm-version` *version*  
&nbsp;&nbsp;&nbsp;&nbsp;目标 EVM 版本。

[output-desc]: https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-api
```
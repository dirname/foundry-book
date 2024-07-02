```markdown
#### 测试选项

`-m` *regex*  
`--match` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行匹配指定正则表达式的测试函数。  
&nbsp;&nbsp;&nbsp;&nbsp;**已弃用：请参见 `--match-test`。**

`--match-test` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行匹配指定正则表达式的测试函数。

`--no-match-test` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行不匹配指定正则表达式的测试函数。

`--match-contract` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行匹配指定正则表达式的合约中的测试。

`--no-match-contract` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行不匹配指定正则表达式的合约中的测试。

`--match-path` *glob*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行匹配指定 glob 模式的源文件中的测试。

`--no-match-path` *glob*  
&nbsp;&nbsp;&nbsp;&nbsp;仅运行不匹配指定 glob 模式的源文件中的测试。

`--debug` *regex*  
&nbsp;&nbsp;&nbsp;&nbsp;在调试器中运行测试。

&nbsp;&nbsp;&nbsp;&nbsp;传递给此标志的参数是您要运行的测试函数的名称，其工作方式与 `--match-test` 相同。

&nbsp;&nbsp;&nbsp;&nbsp;如果多个测试匹配您指定的条件，您必须添加额外的过滤器，直到找到一个测试（参见 `--match-contract` 和 `--match-path`）。

&nbsp;&nbsp;&nbsp;&nbsp;匹配的测试将在调试器中打开，无论测试结果如何。

&nbsp;&nbsp;&nbsp;&nbsp;如果匹配的测试是模糊测试，则它将在第一个失败案例中打开调试器。如果模糊测试没有失败，它将在最后一个模糊案例中打开调试器。

&nbsp;&nbsp;&nbsp;&nbsp;有关更精细的模糊案例控制，请参见 [`forge debug`](./forge-debug.md)。

`--gas-report`  
&nbsp;&nbsp;&nbsp;&nbsp;打印 gas 报告。

`--allow-failure`  
&nbsp;&nbsp;&nbsp;&nbsp;即使测试失败也以代码 0 退出。

`--fail-fast`  
&nbsp;&nbsp;&nbsp;&nbsp;在第一次失败后停止运行测试。

`--etherscan-api-key` *key*  
&nbsp;&nbsp;&nbsp;&nbsp;Etherscan API 密钥。如果设置，且同时设置了 `--fork-url`，则使用 Etherscan 解码 traces。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量：`ETHERSCAN_API_KEY`
```
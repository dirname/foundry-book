```metadata
id: watch-options
```

#### 监视选项

`-w` [*路径...*]  
`--watch` [*路径...*]  
&nbsp;&nbsp;&nbsp;&nbsp;监视特定的文件或文件夹。

&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，监视项目的源目录。

`-d` *延迟*  
`--delay` *延迟*  
&nbsp;&nbsp;&nbsp;&nbsp;文件更新防抖延迟。

&nbsp;&nbsp;&nbsp;&nbsp;在延迟期间，传入的更改事件会被累积，只有在延迟结束后才会采取行动。  
&nbsp;&nbsp;&nbsp;&nbsp;注意，这并不意味着命令会启动：如果给出了 `--no-restart`，并且命令已经在运行，行动的结果将是什么都不做。

&nbsp;&nbsp;&nbsp;&nbsp;默认值为 50 毫秒。默认情况下解析为十进制秒，但使用带有 `ms` 后缀的整数可能更方便。

&nbsp;&nbsp;&nbsp;&nbsp;在使用 `--poll` 模式时，你可能需要更大的持续时间，以避免过度占用磁盘 I/O。

`--no-restart`  
&nbsp;&nbsp;&nbsp;&nbsp;在命令运行时不重启。

`--run-all`  
&nbsp;&nbsp;&nbsp;&nbsp;当发生更改时，显式地重新运行所有文件的命令。
```markdown
#### 项目选项

`--build-info`  
&nbsp;&nbsp;&nbsp;&nbsp;生成构建信息文件。

`--build-info-path` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;输出路径到构建信息文件将被写入的目录。

`--root` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录，或者当前工作目录。

`-C` *路径*  
`--contracts` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;合约源目录。  
&nbsp;&nbsp;&nbsp;&nbsp;环境变量: `DAPP_SRC`

`--lib-paths` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;库文件夹的路径。

`-R` *重映射*  
`--remappings` *重映射*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的重映射。

&nbsp;&nbsp;&nbsp;&nbsp;参数是以逗号分隔的重映射列表，格式为 `<源>=<目标>`。

`--cache-path` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;编译器缓存的路径。

`--config-path` *文件*  
&nbsp;&nbsp;&nbsp;&nbsp;配置文件的路径。

`--hh`  
`--hardhat`  
&nbsp;&nbsp;&nbsp;&nbsp;这是一个便捷标志，等同于传递 `--contracts contracts --lib-paths node-modules`。
```
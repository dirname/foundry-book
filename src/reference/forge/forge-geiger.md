## forge geiger

### 名称

forge-geiger - 检测 foundry 项目及其依赖项中不安全作弊代码的使用情况。

### 概要

``forge geiger`` [*选项*] [*路径*]

### 描述

检测 foundry 项目及其依赖项中不安全作弊代码的使用情况。

### 选项

`--root` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录，或者当前工作目录。

`--check`      
&nbsp;&nbsp;&nbsp;&nbsp;以 'check' 模式运行。如果没有发现不安全的作弊代码，则退出代码为 0。如果检测到不安全的作弊代码，则退出代码为 1。

`--full`  
&nbsp;&nbsp;&nbsp;&nbsp;打印所有文件的完整报告，即使没有发现不安全的函数。

{{#include common-options.md}}

### 参见

[forge](./forge.md)
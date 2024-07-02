## forge tree

### 名称

forge-tree - 显示项目依赖图的树形可视化。

### 概要

``forge tree`` [*选项*]

### 描述

显示项目依赖图的可视化。

```ignore
{{#include ../../output/forge_tree/forge-tree:all}}
```

### 选项

#### 扁平化选项

`--charset` *字符集*  
&nbsp;&nbsp;&nbsp;&nbsp;在输出中使用的字符集：utf8, ascii。默认：utf8

`--no-dedupe`  
&nbsp;&nbsp;&nbsp;&nbsp;不进行去重（重复所有共享依赖）

{{#include project-options.md}}

{{#include common-options.md}}

### 另见

[forge](./forge.md)
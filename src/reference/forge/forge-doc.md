## forge doc

### 名称

forge-doc - 生成 Solidity 源文件的文档。

### 概要

``forge doc`` [*选项*]

### 描述

从 Solidity 源文件生成并构建一个 mdbook。

### 选项

`--root` *路径*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录，或者当前工作目录。

`--out` *路径*
&nbsp;&nbsp;&nbsp;&nbsp;生成 mdbook 的输出路径。默认情况下，它是项目根目录下的 `docs/`。

`--build`
&nbsp;&nbsp;&nbsp;&nbsp;从生成的文件构建 `mdbook`。

`--serve`
&nbsp;&nbsp;&nbsp;&nbsp;在本地提供文档服务。

`--hostname` *主机名*
&nbsp;&nbsp;&nbsp;&nbsp;用于提供文档的主机名。需要 `--serve`。

`--port` *端口*
&nbsp;&nbsp;&nbsp;&nbsp;用于提供文档的端口。需要 `--serve`。

{{#include common-options.md}}

### 示例

1. 生成文档。
    ```sh
    forge doc
    ```
2. 生成并构建文档，指定输出目录。
    ```sh
    forge doc --build --out ./documentation
    ```
3. 生成并在本地端口 4000 上提供文档服务。
    ```sh
    forge doc --serve --port 4000
    ```

### 另请参阅

[文档配置](../config/doc-generator.md)
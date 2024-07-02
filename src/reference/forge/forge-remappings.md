## forge remappings

### NAME

forge-remappings - 获取项目自动推断的重映射。

### SYNOPSIS

``forge remappings`` [*options*]

### DESCRIPTION

获取项目自动推断的重映射。

### OPTIONS

#### 项目选项

`--root` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的根路径。默认情况下，这是当前 git 仓库的根目录，或者当前工作目录。

`--lib-path` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;库文件夹的路径。

{{#include common-options.md}}

### EXAMPLES

1. 从推断的重映射创建一个 `remappings.txt` 文件：
    ```sh
    forge remappings > remappings.txt
    ```

### SEE ALSO

[forge](./forge.md)
#### 缓存选项

`--force`  
&nbsp;&nbsp;&nbsp;&nbsp;清除缓存和 artifacts 文件夹并重新编译。

#### 链接器选项

`--libraries` *libraries*  
&nbsp;&nbsp;&nbsp;&nbsp;设置预链接的库。

&nbsp;&nbsp;&nbsp;&nbsp;参数必须采用 `<重映射的库路径>:<库名称>:<地址>` 格式，例如 `src/Contract.sol:Library:0x...`。

&nbsp;&nbsp;&nbsp;&nbsp;也可以在配置文件中设置为 `libraries = ["<路径>:<库名称>:<地址>"]`。

{{#include compiler-options.md}}

{{#include project-options.md}}

`-o` *path*  
`--out` *path*  
&nbsp;&nbsp;&nbsp;&nbsp;项目的 artifacts 目录。

`--silent`   
&nbsp;&nbsp;&nbsp;&nbsp;抑制所有输出。
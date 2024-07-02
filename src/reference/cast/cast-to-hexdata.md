## cast to-hexdata

### NAME

cast-to-hexdata - 将输入规范化为小写、0x前缀的十六进制。

### SYNOPSIS

``cast to-hexdata`` [*options*] *input*

### DESCRIPTION

将输入规范化为小写、0x前缀的十六进制。

输入数据（*input*）可以是以下几种形式：

- 带有或不带有0x前缀的混合大小写十六进制。
- 应该通过`:`分隔并连接的0x前缀十六进制。
- 包含十六进制的文件的绝对路径。
- 一个`@tag`，其中tag在环境变量中定义。

### OPTIONS

{{#include common-options.md}}

### EXAMPLES

1. 添加0x前缀：
    ```sh
    cast to-hexdata deadbeef
    ```

2. 连接十六进制值：
    ```sh
    cast to-hexdata "deadbeef:0xbeef"
    ```

3. 规范化`MY_VAR`中的十六进制值：
    ```sh
    cast to-hexdata "@MY_VAR"
    ```

### SEE ALSO

[cast](./cast.md)
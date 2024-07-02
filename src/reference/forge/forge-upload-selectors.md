## forge upload-selectors

### NAME

forge-upload-selectors - 将给定合约的 ABI 上传到 https://sig.eth.samczsun.com 函数选择器数据库。

### SYNOPSIS

``forge upload-selectors`` [*options*] *contract*

### DESCRIPTION

将给定合约的 ABI 上传到 https://sig.eth.samczsun.com 函数选择器数据库。

### OPTIONS

{{#include project-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 上传 ABI 到选择器数据库
    ```sh
    forge upload-selectors LinearVestingVault
    ```
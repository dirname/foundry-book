## `forge cache clean`

### 名称

forge-cache-clean - 清除 `~/.foundry` 中的缓存数据。

### 概要

`forge cache clean` [*选项*] [*--*] [*链..*]

### 描述

删除 `~/.foundry/cache` 文件夹中的文件，该文件夹用于缓存 Etherscan 验证状态和区块数据。

### 选项

`-b`  
`--blocks`  
&nbsp;&nbsp;&nbsp;&nbsp;一个或多个区块号，用逗号分隔，无空格

`--etherscan`
&nbsp;&nbsp;&nbsp;&nbsp;一个布尔标志，指定仅删除缓存中的区块浏览器部分

{{#include common-options.md}}

### 示例

1. 删除整个缓存（也是 `forge cache clean` 的别名）

   ```sh
   forge cache clean all
   ```

2. 删除整个区块浏览器缓存

   ```sh
   forge cache clean all --etherscan
   ```

3. 删除特定链的缓存数据，按名称

   ```sh
   forge cache clean rinkeby
   ```

4. 删除特定链上特定区块号的缓存数据。如果 `chain` 是 `all`，则不适用

   ```sh
   forge cache clean rinkeby -b 150000
   ```

5. 删除特定链的区块浏览器缓存数据。如果指定了 `--blocks`，则不适用

   ```sh
   forge cache clean rinkeby --etherscan
   ```

6. 指定多条链

   ```sh
   forge cache clean rinkeby mainnet
   ```

7. 指定多个区块
   ```sh
   forge cache clean rinkeby --blocks 530000,9000000,9200000
   ```

### 另见
[forge](./forge.md), [forge cache](./forge-cache.md)
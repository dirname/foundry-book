## 文件作弊码

### 签名

```solidity
// 读取文件的全部内容到字符串，(路径) => (数据)
function readFile(string calldata) external returns (string memory);
/// 以二进制形式读取文件的全部内容。`路径`相对于项目根目录。
function readFileBinary(string calldata path) external view returns (bytes memory data);
/// 递归读取给定路径的目录，最多到`最大深度`。
/// `最大深度`默认为1，表示只返回给定目录的直接子目录。
/// 如果`跟随链接`为真，则跟随符号链接。
function readDir(string calldata path) external view returns (DirEntry[] memory entries);
// 读取文件的下一行到字符串，(路径) => (行)
function readLine(string calldata) external returns (string memory);
/// 读取符号链接，返回链接指向的路径。
/// 此作弊码在以下情况下会回滚，但不限于这些情况：
/// - `路径`不是符号链接。
/// - `路径`不存在。
function readLink(string calldata linkPath) external view returns (string memory targetPath);
// 写入数据到文件，如果不存在则创建文件，如果存在则完全替换其内容。
// (路径, 数据) => ()
function writeFile(string calldata, string calldata) external;
// 写入行到文件，如果不存在则创建文件。
// (路径, 数据) => ()
function writeLine(string calldata, string calldata) external;
// 关闭文件以进行读取，重置偏移量，允许从开头用readLine读取。
// (路径) => ()
function closeFile(string calldata) external;
// 删除文件。此作弊码在以下情况下会回滚，但不限于这些情况：
// - 路径指向目录。
// - 文件不存在。
// - 用户缺乏删除文件的权限。
// (路径) => ()
function removeFile(string calldata) external;
// 如果给定路径指向现有实体，则返回真，否则返回假
// (路径) => (布尔值)
function exists(string calldata) external returns (bool);
// 如果路径存在于磁盘上并且指向常规文件，则返回真，否则返回假
// (路径) => (布尔值)
function isFile(string calldata) external returns (bool);
// 如果路径存在于磁盘上并且指向目录，则返回真，否则返回假
// (路径) => (布尔值)
function isDir(string calldata) external returns (bool);
```

### 描述

这些由 [forge-std](https://github.com/foundry-rs/forge-std) 提供的作弊码可用于文件系统操作。

默认情况下，文件系统访问是被禁止的，需要在 `foundry.toml` 中设置 `fs_permission`：

```toml
# 配置作弊码的权限，这些作弊码涉及文件系统操作，如 `vm.writeFile`
# `access` 限制通过作弊码访问 `路径` 的方式
#    `read-write` | `true`   => 允许 `读` + `写` 访问 (`vm.readFile` + `vm.writeFile`)
#    `none`| `false` => 无访问权限
#    `read` => 仅读访问 (`vm.readFile`)
#    `write` => 仅写访问 (`vm.writeFile`)
# `allowed_paths` 进一步列出被考虑的路径，例如 `./` 表示项目根目录
# 默认情况下 _无_ 文件系统访问权限，且 _无_ 路径被允许
# 以下示例仅启用对项目目录的读访问权限：
#       `fs_permissions = [{ access = "read", path = "./"}]`
fs_permissions = [] # 默认：所有文件作弊码都被禁用
```

### 示例

追加一行到文件，如果文件不存在则创建文件

这需要对文件 / 项目根目录的读访问权限

```toml
fs_permissions = [{ access = "read", path = "./"}]
```

```solidity
string memory path = "output.txt";

string memory line1 = "first line";
vm.writeLine(path, line1);

string memory line2 = "second line";
vm.writeLine(path, line2);
```

写入和读取文件

这需要对文件 / 项目根目录的读写访问权限：

```toml
fs_permissions = [{ access = "read-write", path = "./"}]
```

```solidity
string memory path = "file.txt";
string memory data = "hello world";
vm.writeFile(path, data);

assertEq(vm.readFile(path), data);
```

删除文件

这需要对文件 / 项目根目录的写访问权限：

```toml
fs_permissions = [{ access = "write", path = "./"}]
```

```solidity
string memory path = "file.txt";
vm.removeFile(path);

assertFalse(vm.exists(validPath));
```

验证文件系统路径是否有效

```solidity
// 验证路径 'foo/files/bar.txt' 是否存在
string memory validPath = "foo/files/bar.txt";
assertTrue(vm.exists(validPath));
```

验证文件系统路径是否指向文件或目录

```solidity
// 验证路径 'foo/file/bar.txt' 是否指向文件
string memory validFilePath = "foo/files/bar.txt";
assertTrue(vm.isFile(validFilePath));

// 验证 'foo/file' 是否指向目录
string memory validDirPath = "foo/files";
assertTrue(vm.isDir(validDirPath));
```
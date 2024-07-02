## `writeJson`

### 签名

```solidity
function writeJson(string calldata json, string calldata path) external;

function writeJson(string calldata json, string calldata path, string calldata valueKey) external;
```

### 描述

将序列化的 JSON 对象写入文件。

参数 `json` 必须是一个字符串化的 JSON 对象。例如：

```text
{ "boolean": true, "number": 342, "object": { "title": "finally json serialization" } }
```

这通常通过 [serializeJson](./serialize-json.md) 构建。

参数 `path` 是要写入的 JSON 文件的路径。

如果没有提供 `valueKey`，则 JSON 对象将写入一个新文件。如果文件已存在，它将被覆盖。

如果提供了 `valueKey`，则文件必须已经存在并且是一个有效的 JSON 文件。该文件中的对象将通过将 *JSON 路径* `valueKey` 处的值替换为 JSON 对象 `json` 来更新。

这对于在不首先解析然后重新序列化文件的情况下替换 JSON 文件中的某些值非常有用。请注意，JSON 路径必须指示现有键，因此这种方式无法添加新键。

**记住：** 文件路径 `path` 需要在允许的路径中。更多信息请阅读 [File cheatcodes](./fs.md)。

#### JSON 路径

让我们考虑以下 JSON 对象：

```json
{
  "boolean": true,
  "number": 342,
  "obj1": {
    "aNumber": 123,
    "obj2": {
      "aNumber": 123,
      "obj3": {
        "veryDeep": 13371337
      }
    }
  }
}
```

根对象总是被假定，因此我们可以通过以点 (`.`) 开始路径来引用其子对象。例如，`.boolean`、`.number` 和 `.obj1`。
我们可以深入到任意深度：`.obj1.aNumber` 或 `.obj1.obj2.aNumber`。
我们甚至可以在子树中搜索键：`.obj1..veryDeep` 或只是 `..veryDeep`，因为没有歧义。

请参见示例以查看实际操作。

### 示例

#### 一个简单的示例

```solidity
string memory jsonObj = '{ "boolean": true, "number": 342, "myObject": { "title": "finally json serialization" } }';
vm.writeJson(jsonObj, "./output/example.json");

// 将 `myObject` 的值替换为一个新的对象
string memory newJsonObj = '{ "aNumber": 123, "aString": "asd" }';
vm.writeJson(newJsonObj, "./output/example.json", ".myObject");

// 将新对象中的 `aString` 的值替换
vm.writeJson("my new string", "./output/example.json", ".myObject.aString");

// 这里是 example.json 的内容：
// 
// {
//   "boolean": true,
//   "number": 342,
//   "myObject": {
//     "aNumber": 123,
//     "aString": "my new string"
//   }
// }
```

#### 一个更复杂的示例

```solidity
string memory jsonObj = '{ "boolean": true, "number": 342, "obj1": { "foo": "bar" } }';
vm.writeJson(jsonObj, "./output/example2.json");

string memory jsonObj2 = '{ "aNumber": 123, "obj2": {} }';
vm.writeJson(jsonObj2, "./output/example2.json", ".obj1");

string memory jsonObj3 = '{ "aNumber": 123, "obj3": { "veryDeep": 3 } }';
vm.writeJson(jsonObj3, "./output/example2.json", ".obj1.obj2");

// 这里是 example2.json 的内容：
//
// {
//   "boolean": true,
//   "number": 342,
//   "obj1": {
//     "aNumber": 123,
//     "obj2": {
//       "aNumber": 123,
//       "obj3": {
//         "veryDeep": 3
//       }
//     }
//   }
// }

// 注意，在这种情况下，JSON 对象只是值 13371337。
vm.writeJson("13371337", "./output/example2.json", "..veryDeep");

// 这里是最终的 example2.json 内容：
//
// {
//   "boolean": true,
//   "number": 342,
//   "obj1": {
//     "aNumber": 123,
//     "obj2": {
//       "aNumber": 123,
//       "obj3": {
//         "veryDeep": 13371337
//       }
//     }
//   }
// }
```

### 另请参阅

- [serializeJson](./serialize-json.md)
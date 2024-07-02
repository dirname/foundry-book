## `serializeJson`

### 签名

```solidity
function serializeJson(string calldata objectKey, string calldata value)
    external
    returns (string memory json);

function serializeBool(string calldata objectKey, string calldata valueKey, bool value)
    external
    returns (string memory json);

function serializeUint(string calldata objectKey, string calldata valueKey, uint256 value)
    external
    returns (string memory json);

function serializeInt(string calldata objectKey, string calldata valueKey, int256 value)
    external
    returns (string memory json);

function serializeAddress(string calldata objectKey, string calldata valueKey, address value)
    external
    returns (string memory json);

function serializeBytes32(string calldata objectKey, string calldata valueKey, bytes32 value)
    external
    returns (string memory json);

function serializeString(string calldata objectKey, string calldata valueKey, string calldata value)
    external
    returns (string memory json);

function serializeBytes(string calldata objectKey, string calldata valueKey, bytes calldata value)
    external
    returns (string memory json);

function serializeBool(string calldata objectKey, string calldata valueKey, bool[] calldata values)
    external
    returns (string memory json);

function serializeUint(string calldata objectKey, string calldata valueKey, uint256[] calldata values)
    external
    returns (string memory json);

function serializeInt(string calldata objectKey, string calldata valueKey, int256[] calldata values)
    external
    returns (string memory json);

function serializeAddress(string calldata objectKey, string calldata valueKey, address[] calldata values)
    external
    returns (string memory json);

function serializeBytes32(string calldata objectKey, string calldata valueKey, bytes32[] calldata values)
    external
    returns (string memory json);

function serializeString(string calldata objectKey, string calldata valueKey, string[] calldata values)
    external
    returns (string memory json);

function serializeBytes(string calldata objectKey, string calldata valueKey, bytes[] calldata values)
    external
    returns (string memory json);
```

### 描述

将值序列化为字符串化的 JSON 对象。

### 工作原理

用户序列化 JSON 文件的值，最后将该对象写入文件。用户需要传递：

- 一个用于 _对象_ 的键，该值应序列化到该对象。这使用户能够并行序列化多个对象
- 一个用于 _值_ 的键，该键将在 JSON 文件中作为其键
- 要序列化的值

`serializeJson` 函数是一个例外，它只接收一个 `objectKey` 和一个 JSON 字符串 `value`。这允许用户序列化现有的 JSON 对象，并直接将其分配给提供的 `objectKey`。如果 `objectKey` 已在使用中，则整个序列化的 JSON 将被覆盖。

键不需要是某种特定的形式。它们是 `string` 类型，以便于直观的人类解释。从语义上讲，它们除了用作键之外并不重要。

作弊码返回正在序列化的 JSON 对象 **到该点为止**。这样用户可以序列化内部 JSON 对象，然后将它们序列化到更大的 JSON 对象中，使用户能够创建任意的 JSON 对象。

最后，用户通过使用 [writeJson](./write-json.md) 将 JSON 对象写入 JSON 文件。
或者，用户可以通过使用 [writeToml](./write-toml.md) 将 JSON 对象写入 TOML 文件。

**记住：** 文件路径需要在允许的路径中。更多信息请阅读 [文件作弊码](./fs.md)。

### 示例

假设我们要将以下 JSON 写入文件：

{ "boolean": true, "number": 342, "object": { "title": "finally json serialization" } }

```solidity
string memory obj1 = "some key";
vm.serializeBool(obj1, "boolean", true);
vm.serializeUint(obj1, "number", uint256(342));

string memory obj2 = "some other key";
string memory output = vm.serializeString(obj2, "title", "finally json serialization");

// 重要：这之所以有效，是因为 `serializeString` 首先尝试将 `output` 解释为
//   字符串化的 JSON 对象。如果解析失败，则将其视为普通字符串。
//   例如，等于 '{ "ok": "asd" }' 的 `output` 将生成一个对象，但
//   等于 '"ok": "asd" }' 的 `output` 将只生成一个普通字符串。
string memory finalJson = vm.serializeString(obj1, "object", output);

vm.writeJson(finalJson, "./output/example.json");
```

### 另请参阅

- [writeJson](./write-json.md)
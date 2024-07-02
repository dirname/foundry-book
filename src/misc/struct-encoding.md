## 结构体编码

结构体是用户定义的类型，可以组合多个变量：

```solidity
struct MyStruct {
    address addr;
    uint256 amount;
}
```

只有新的 [ABI 编码器 v2](https://docs.soliditylang.org/zh/latest/layout-of-source-files.html#abi-coder-pragma) 才能编码和解码任意嵌套的数组和结构体。自 Solidity 0.8.0 起，它默认激活，在此之前需要通过 `pragma experimental ABIEncoderV2` 激活。

Solidity 结构体映射到 ABI 类型 "tuple"。有关 Solidity 类型如何映射到 ABI 类型的更多信息，请参阅 Solidity 文档中的 [Mapping Solidity to ABI types](https://docs.soliditylang.org/zh/latest/abi-spec.html#mapping-solidity-to-abi-types)。

因此，结构体作为元组进行编码和解码。所以我们定义的结构体 `MyStruct` 在 ABI 中映射为元组 `(address,uint256)`。

让我们看看这在合约中是如何工作的：

```solidity
pragma solidity =0.8.15;

contract Test {
    struct MyStruct {
        address addr;
        uint256 amount;
    }
    function f(MyStruct memory t) public pure {}
}
```

该合约中函数 `f` 的 ABI 是：

```json
{
	"inputs": [
		{
			"components": [
				{
					"internalType": "address",
					"name": "addr",
					"type": "address"
				},
				{
					"internalType": "uint256",
					"name": "amount",
					"type": "uint256"
				}
			],
			"internalType": "struct Test.MyStruct",
			"name": "t",
			"type": "tuple"
		}
	],
	"name": "f",
	"outputs": [],
	"stateMutability": "pure",
	"type": "function"
}
```

这表示：函数 `f` 接受一个类型为 `tuple` 的输入，包含两个类型分别为 `address` 和 `uint256` 的组件。

**嵌套结构体编码：**
以下是一个更复杂的嵌套结构体示例：

```solidity
pragma solidity 0.8.21;

contract Test {
    struct nestedStruct {
        address addr;
        uint256 amount;
    }

    struct MyStruct {
        string nestedStructName;
        uint256 nestedStructCount;
        nestedStruct _nestedStruct;
    }

    function f(MyStruct memory t) public pure {}
}
```
该合约中函数 `f` 的 ABI 是：

```json
{
    "inputs": [
        {
            "name": "t",
            "type": "tuple",
            "internalType": "struct Test.MyStruct",
            "components": [
                {
                    "name": "nestedStructName",
                    "type": "string",
                    "internalType": "string"
                },
                {
                    "name": "nestedStructCount",
                    "type": "uint256",
                    "internalType": "uint256"
                },
                {
                    "name": "_nestedStruct",
                    "type": "tuple",
                    "internalType": "struct Test.nestedStruct",
                    "components": [
                        {
                            "name": "addr",
                            "type": "address",
                            "internalType": "address"
                        },
                        {
                            "name": "amount",
                            "type": "uint256",
                            "internalType": "uint256"
                        }
                    ]
                }
            ]
        }
    ],
    "name": "f",
    "outputs": [],
    "stateMutability": "pure",
    "type": "function"
}
```
这表示：函数 `f` 接受一个类型为元组的输入，包含三个组件：一个字符串、一个 uint256 和一个表示嵌套结构体的元组，该元组包含类型为 address 的 addr 和类型为 uint256 的 amount。

要编码 `MyStruct` 并将其作为参数传递给函数 `f`：
```bash
cast abi-encode "f((string,uint256,(address,uint256)))" "(example,1,(0x...,1))"
```

要部署一个接受 `MyStruct` 作为参数的合约：
```bash
forge create src/Test.sol:Test --constructor-args "(example,1,(0x...,1))"
```
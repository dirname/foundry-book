## `getCode`

### 签名

```solidity
function getCode(string calldata) external returns (bytes memory);
```

### 描述

返回项目中给定合约路径的**创建**字节码。

calldata 参数可以是 `ContractFile.sol`（如果文件名和合约名相同）、`ContractFile.sol:ContractName` 或相对于项目根目录的工件路径。

> ℹ️ **注意**
>
> `getCode` 需要读取输出目录的权限，请参阅 [文件作弊码](./fs.md)。
>
> 要授予读取权限，请在 `foundry.toml` 中设置 `fs_permissions = [{ access = "read", path = "./out"}]`。

### 示例

```solidity
MyContract myContract = new MyContract(arg1, arg2);

// 让我们用 `getCode` 做同样的事情
bytes memory args = abi.encode(arg1, arg2);
bytes memory bytecode = abi.encodePacked(vm.getCode("MyContract.sol:MyContract"), args);
address anotherAddress;
assembly {
    anotherAddress := create(0, add(bytecode, 0x20), mload(bytecode))
}

assertEq0(address(myContract).code, anotherAddress.code); // [PASS]
```

通过结合 `getCode` 和 [`etch`](./etch.md) 将合约部署到任意地址

```solidity
// 部署
bytes memory args = abi.encode(arg1, arg2);
bytes memory bytecode = abi.encodePacked(vm.getCode("MyContract.sol:MyContract"), args);
address deployed;
assembly {
    deployed := create(0, add(bytecode, 0x20), mload(bytecode))
}

// 设置任意地址的字节码
vm.etch(targetAddr, deployed.code);
```


### 支持的格式

您可以通过合约路径或合约名称获取工件。还支持获取特定版本的工件。如果未提供，作弊码将默认为正在执行的测试版本或工件编译的唯一版本。
```solidity
vm.getCode("MyContract.sol:MyContract");
vm.getCode("MyContract");
vm.getCode("MyContract.sol:0.8.18");
vm.getCode("MyContract:0.8.18");
```

### 另请参阅

[`getDeployedCode`](./get-deployed-code.md)
[`eth`](./etch.md)

Forge 标准库

[`deployCode`](../reference/forge-std/deployCode.md)
[`deployCodeTo`](../reference/forge-std/deployCodeTo.md)

[forge-std]: ../reference/forge-std
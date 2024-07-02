## `getDeployedCode`

### 签名

```solidity
function getDeployedCode(string calldata) external returns (bytes memory);
```

### 描述

这个作弊码与 [`getCode`](./get-code.md) 类似，但只返回项目中合约的**已部署**字节码（即运行时字节码），给定合约的路径。

这个作弊码的主要用例是作为将无状态合约部署到任意地址的快捷方式。

调用参数可以是 `ContractFile.sol`（如果文件名和合约名相同）、`ContractFile.sol:ContractName` 或项目根目录相对路径的构件路径。

> ℹ️ **注意**
>
> `getDeployedCode` 需要读取输出目录的权限，请参阅 [文件作弊码](./fs.md)。
>
> 要授予读取权限，请在 `foundry.toml` 中设置 `fs_permissions = [{ access = "read", path = "./out"}]`。

### 示例

使用 `getDeployedCode` 和 [`etch`](./etch.md) 在任意地址部署一个无状态合约。

```solidity
// 我们希望在特定地址部署的无状态合约
contract Override {
    event Payload(address sender, address target, bytes data);

    function emitPayload(
        address target, bytes calldata message
    ) external payable returns (uint256) {
        emit Payload(msg.sender, target, message);
        return 0;
    }
}

// 获取**已部署字节码**
bytes memory code = vm.getDeployedCode("Override.sol:Override");

// 设置任意地址的代码
address overrideAddress = address(64);
vm.etch(overrideAddress, code);
assertEq(overrideAddress.code, code);
```

### 支持的格式

你可以通过合约路径或合约名称获取构件。也支持获取特定版本的构件。如果未提供，作弊码将默认使用正在执行测试的版本或构件编译的唯一版本。
```solidity
vm.getDeployedCode("MyContract.sol:MyContract");
vm.getDeployedCode("MyContract");
vm.getDeployedCode("MyContract.sol:0.8.18");
vm.getDeployedCode("MyContract:0.8.18");
```

### 另请参阅

Forge 标准库

[`getCode`](./get-code.md)
[`etch`](./etch.md)

[forge-std]: ../reference/forge-std
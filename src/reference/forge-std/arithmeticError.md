## `arithmeticError`

### 签名

```solidity
stdError.arithmeticError
```

### 描述

当算术操作失败时，Solidity 内部的错误，例如下溢和上溢。

### 示例

假设我们有一个基本的保险库合约，可以存储一些代币（`wmdToken`）：

```solidity
contract BasicVault {

    IERC20 public immutable wmdToken;   
    mapping(address => uint) public balances;

    event Deposited(address indexed from, uint amount);
    event Withdrawal(address indexed from, uint amount);

    constructor(IERC20 wmdToken_){
        wmdToken = wmdToken_;
    }

    function deposit(uint amount) external {    
        balances[msg.sender] += amount;
        bool success = wmdToken.transferFrom(msg.sender, address(this), amount);
        require(success, "存款失败！"); 
        emit Deposited(msg.sender, amount);
    }

    function withdraw(uint amount) external {      
        balances[msg.sender] -= amount;
        bool success = wmdToken.transfer(msg.sender, amount);
        require(success, "取款失败！");
        emit Withdrawal(msg.sender, amount);
    }
}
```

我们有一个测试函数，确保用户无法提取超过其存款的代币，如下所示：

```solidity
function testUserCannotWithdrawExcessOfDeposit() public {
    vm.prank(user);
    vm.expectRevert(stdError.arithmeticError);
    vault.withdraw(userTokens + 100*10**18);
}
```

1. 用户在保险库合约中存入了数量为 `userTokens` 的代币。
2. 用户尝试提取超过其存款数量的代币。
3. 这会导致下溢错误，因为 `balances[msg.sender] -= amount;` 会计算出一个负值。

为了捕获错误“算术上溢/下溢”，我们在预期会导致下溢的函数调用之前插入 `vm.expectRevert(stdError.arithmeticError)`。
## `fail`

### 签名

```solidity
function fail(string memory err) internal virtual;
```

### 描述

如果命中某个分支或执行点，则使用消息使测试失败。

### 示例

```solidity
function test() external {
    for(uint256 place; place < 10; ++i){
        if(game.leaderboard(place) == address(this)) return;
    }
    fail("不在前10名。");
}
```
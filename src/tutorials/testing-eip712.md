## 测试 EIP-712 签名

### 介绍

[EIP-712](https://eips.ethereum.org/EIPS/eip-712) 引入了在链下签署交易的能力，其他用户可以在链上执行这些交易。一个常见的例子是 [EIP-2612](https://eips.ethereum.org/EIPS/eip-2612) 无 gas 代币批准。

传统上，设置用户或合约从所有者余额中转移 ERC-20 代币的允许额度需要所有者在链上提交批准。由于这证明是糟糕的用户体验，DAI 引入了 ERC-20 `permit`（后来标准化为 EIP-2612），允许所有者在链下签署批准，然后支出者（或任何人！）可以在链上提交 `transferFrom` 之前执行。

本指南将介绍如何使用 Foundry 在 Solidity 中测试这种模式。

### 深入探讨

首先，我们将介绍一个基本的代币转移：

- 所有者在链下签署批准
- 支出者在链上调用 `permit` 和 `transferFrom`

我们将使用 [Solmate 的 ERC-20](https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC20.sol)，因为 EIP-712 和 EIP-2612 的功能已经包含在内。如果你还没有看过完整的合约，可以先浏览一下 - 这里是 `permit` 的实现：

```solidity
    /*//////////////////////////////////////////////////////////////
                             EIP-2612 LOGIC
    //////////////////////////////////////////////////////////////*/

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) public virtual {
        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

        // Unchecked because the only math done is incrementing
        // the owner's nonce which cannot realistically overflow.
        unchecked {
            address recoveredAddress = ecrecover(
                keccak256(
                    abi.encodePacked(
                        "\x19\x01",
                        DOMAIN_SEPARATOR(),
                        keccak256(
                            abi.encode(
                                keccak256(
                                    "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
                                ),
                                owner,
                                spender,
                                value,
                                nonces[owner]++,
                                deadline
                            )
                        )
                    )
                ),
                v,
                r,
                s
            );

            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

            allowance[recoveredAddress][spender] = value;
        }

        emit Approval(owner, spender, value);
    }
```

我们还将使用一个自定义的 `SigUtils` 合约来帮助创建、哈希和链下签署批准。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

contract SigUtils {
    bytes32 internal DOMAIN_SEPARATOR;

    constructor(bytes32 _DOMAIN_SEPARATOR) {
        DOMAIN_SEPARATOR = _DOMAIN_SEPARATOR;
    }

    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH =
        0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9;

    struct Permit {
        address owner;
        address spender;
        uint256 value;
        uint256 nonce;
        uint256 deadline;
    }

    // 计算 permit 的哈希
    function getStructHash(Permit memory _permit)
        internal
        pure
        returns (bytes32)
    {
        return
            keccak256(
                abi.encode(
                    PERMIT_TYPEHASH,
                    _permit.owner,
                    _permit.spender,
                    _permit.value,
                    _permit.nonce,
                    _permit.deadline
                )
            );
    }

    // 计算用于恢复签名者的完整编码 EIP-712 消息的哈希
    function getTypedDataHash(Permit memory _permit)
        public
        view
        returns (bytes32)
    {
        return
            keccak256(
                abi.encodePacked(
                    "\x19\x01",
                    DOMAIN_SEPARATOR,
                    getStructHash(_permit)
                )
            );
    }
}
```

### 处理动态值

虽然上面 getStructHash() 函数中传递的 Permit 结构体不包含任何动态值类型，但如果你使用它们，重要的是要记住 'bytes' 和 'string' 类型必须编码为其内容的 'keccak256' 哈希。更多关于 [EIP 712 规范的这方面内容](https://github.com/ethereum/EIPs/blob/8061f8e2243eaae829d1fa91f7a763c889aca371/EIPS/eip-712.md?plain=1#L135)。

**设置**

- 部署一个模拟 ERC-20 代币和带有代币 EIP-712 域分隔符的 `SigUtils` 助手
- 创建私钥来模拟所有者和支出者
- 使用 `vm.addr` [作弊码](https://book.getfoundry.sh/cheatcodes/addr.html) 导出他们的地址
- 为所有者铸造一个测试代币

```solidity

contract Token_ERC20 is MockERC20 {
    constructor(string memory name, string memory symbol, uint8 decimals) {
        initialize(name, symbol, decimals);
    }

    function mint(address to, uint256 value) public virtual {
        _mint(to, value);
    }

    function burn(address from, uint256 value) public virtual {
        _burn(from, value);
    }
}

contract ERC20Test is Test {
    Token_ERC20 internal token;
    SigUtils internal sigUtils;

    uint256 internal ownerPrivateKey;
    uint256 internal spenderPrivateKey;

    address internal owner;
    address internal spender;

    function setUp() public {
        token = new Token_ERC20("Token", "TKN", 18);
        sigUtils = new SigUtils(token.DOMAIN_SEPARATOR());

        ownerPrivateKey = 0xA11CE;
        spenderPrivateKey = 0xB0B;

        owner = vm.addr(ownerPrivateKey);
        spender = vm.addr(spenderPrivateKey);

        token.mint(owner, 1e18);
    }
```

**测试: `permit`**

- 为支出者创建一个批准
- 使用 `sigUtils.getTypedDataHash` 计算其摘要
- 使用所有者的私钥通过 `vm.sign` [作弊码](https://book.getfoundry.sh/cheatcodes/sign.html) 签署摘要
- 存储签名的 `uint8 v, bytes32 r, bytes32 s`
- 使用签署的批准和签名调用 `permit` 以在链上执行批准

```solidity
    function test_Permit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        assertEq(token.allowance(owner, spender), 1e18);
        assertEq(token.nonces(owner), 1);
    }
```

- 确保在过期截止时间、无效签名者、无效随机数和签名重放的调用失败

```solidity
    function testRevert_ExpiredPermit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: token.nonces(owner),
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        vm.warp(1 days + 1 seconds); // 快进一秒超过截止时间

        vm.expectRevert("PERMIT_DEADLINE_EXPIRED");
        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );
    }

    function testRevert_InvalidSigner() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: token.nonces(owner),
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(spenderPrivateKey, digest); // 支出者签署所有者的批准

        vm.expectRevert("INVALID_SIGNER");
        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );
    }

    function testRevert_InvalidNonce() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: 1, // 链上存储的所有者随机数是 0
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        vm.expectRevert("INVALID_SIGNER");
        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );
    }

    function testRevert_SignatureReplay() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        vm.expectRevert("INVALID_SIGNER");
        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );
    }
```

**测试: `transferFrom`**

- 创建、签署并执行支出者的批准
- 使用 `vm.prank` [作弊码](https://book.getfoundry.sh/cheatcodes/prank.html) 作为支出者调用 `tokenTransfer` 以执行转移

```solidity
    function test_TransferFromLimitedPermit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 1e18,
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        vm.prank(spender);
        token.transferFrom(owner, spender, 1e18);

        assertEq(token.balanceOf(owner), 0);
        assertEq(token.balanceOf(spender), 1e18);
        assertEq(token.allowance(owner, spender), 0);
    }

    function test_TransferFromMaxPermit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: type(uint256).max,
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        vm.prank(spender);
        token.transferFrom(owner, spender, 1e18);

        assertEq(token.balanceOf(owner), 0);
        assertEq(token.balanceOf(spender), 1e18);
        assertEq(token.allowance(owner, spender), type(uint256).max);
    }
```

- 确保在无效允许额度和无效余额的调用失败

```solidity
    function testFail_InvalidAllowance() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 5e17, // 仅批准 0.5 个代币
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        vm.prank(spender);
        token.transferFrom(owner, spender, 1e18); // 尝试转移 1 个代币
    }

    function testFail_InvalidBalance() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: spender,
            value: 2e18, // 批准 2 个代币
            nonce: 0,
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        token.permit(
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        vm.prank(spender);
        token.transferFrom(owner, spender, 2e18); // 尝试转移 2 个代币（所有者仅拥有 1 个）
    }
```

### 捆绑示例

这里是一个 [模拟合约](https://github.com/kulkarohan/deposit/blob/main/src/Deposit.sol) 的部分，它只是存入 ERC-20 代币。注意 `deposit` 需要一个初步的 `approve` 或 `permit` 交易才能转移代币，而 `depositWithPermit` 在一个交易中设置允许额度并转移代币。

```solidity
    ///                                                          ///
    ///                           DEPOSIT                        ///
    ///                                                          ///

    /// @notice 存入 ERC-20 代币（需要预批准）
    /// @param _tokenContract ERC-20 代币地址
    /// @param _amount 代币数量
    function deposit(address _tokenContract, uint256 _amount) external {
        ERC20(_tokenContract).transferFrom(msg.sender, address(this), _amount);

        userDeposits[msg.sender][_tokenContract] += _amount;

        emit TokenDeposit(msg.sender, _tokenContract, _amount);
    }

    ///                                                          ///
    ///                      DEPOSIT w/ PERMIT                   ///
    ///                                                          ///

    /// @notice 使用签名批准存入 ERC-20 代币
    /// @param _tokenContract ERC-20 代币地址
    /// @param _amount 转移的代币数量
    /// @param _owner 签署批准的用户
    /// @param _spender 转移代币的用户（即本合约）
    /// @param _value 批准支出者的代币数量
    /// @param _deadline 批准过期的时间戳
    /// @param _v 签名的第 129 字节和链 ID
    /// @param _r 签名的前 64 字节
    /// @param _s 签名的第 64-128 字节
    function depositWithPermit(
        address _tokenContract,
        uint256 _amount,
        address _owner,
        address _spender,
        uint256 _value,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external {
        ERC20(_tokenContract).permit(
            _owner,
            _spender,
            _value,
            _deadline,
            _v,
            _r,
            _s
        );

        ERC20(_tokenContract).transferFrom(_owner, address(this), _amount);

        userDeposits[_owner][_tokenContract] += _amount;

        emit TokenDeposit(_owner, _tokenContract, _amount);
    }
```

**设置**

- 部署 `Deposit` 合约、模拟 ERC-20 代币和带有代币 EIP-712 域分隔符的 `SigUtils` 助手
- 创建一个私钥来模拟所有者（支出者现在是 `Deposit` 地址）
- 使用 `vm.addr` [作弊码](https://book.getfoundry.sh/cheatcodes/addr.html) 导出所有者地址
- 为所有者铸造一个测试代币

```solidity
contract DepositTest is Test {
    Deposit internal deposit;
    MockERC20 internal token;
    SigUtils internal sigUtils;

    uint256 internal ownerPrivateKey;
    address internal owner;

    function setUp() public {
        deposit = new Deposit();
        token = new MockERC20();
        sigUtils = new SigUtils(token.DOMAIN_SEPARATOR());

        ownerPrivateKey = 0xA11CE;
        owner = vm.addr(ownerPrivateKey);

        token.mint(owner, 1e18);
    }
```

**测试: `depositWithPermit`**

- 为 `Deposit` 合约创建一个批准
- 使用 `sigUtils.getTypedDataHash` 计算其摘要
- 使用所有者的私钥通过 `vm.sign` [作弊码](https://book.getfoundry.sh/cheatcodes/sign.html) 签署摘要
- 存储签名的 `uint8 v, bytes32 r, bytes32 s`
  - _注意:_ 可以通过 `bytes signature = abi.encodePacked(r, s, v)` 转换为字节
- 使用签署的批准和签名调用 `depositWithPermit` 以将代币转移到合约中

```solidity
    function test_DepositWithLimitedPermit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: address(deposit),
            value: 1e18,
            nonce: token.nonces(owner),
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        deposit.depositWithPermit(
            address(token),
            1e18,
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        assertEq(token.balanceOf(owner), 0);
        assertEq(token.balanceOf(address(deposit)), 1e18);

        assertEq(token.allowance(owner, address(deposit)), 0);
        assertEq(token.nonces(owner), 1);

        assertEq(deposit.userDeposits(owner, address(token)), 1e18);
    }

    function test_DepositWithMaxPermit() public {
        SigUtils.Permit memory permit = SigUtils.Permit({
            owner: owner,
            spender: address(deposit),
            value: type(uint256).max,
            nonce: token.nonces(owner),
            deadline: 1 days
        });

        bytes32 digest = sigUtils.getTypedDataHash(permit);

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, digest);

        deposit.depositWithPermit(
            address(token),
            1e18,
            permit.owner,
            permit.spender,
            permit.value,
            permit.deadline,
            v,
            r,
            s
        );

        assertEq(token.balanceOf(owner), 0);
        assertEq(token.balanceOf(address(deposit)), 1e18);

        assertEq(token.allowance(owner, address(deposit)), type(uint256).max);
        assertEq(token.nonces(owner), 1);

        assertEq(deposit.userDeposits(owner, address(token)), 1e18);
    }
```

- 确保无效 `permit` 和 `transferFrom` 调用失败，如前所示

### TLDR

使用 Foundry 作弊码 `addr`、`sign` 和 `prank` 在 Foundry 中测试 EIP-712 签名。

所有源代码可以在 [这里](https://github.com/kulkarohan/deposit) 找到。
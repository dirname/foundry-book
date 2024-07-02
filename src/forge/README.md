## Forge 概述

Forge 是 Foundry 附带的一个命令行工具。Forge 用于测试、构建和部署您的智能合约。

---

## Forge Commands

Forge provides a variety of commands to help you manage your smart contracts. Below are some of the most commonly used commands.

### `forge init`

Initializes a new Forge project.

```sh
forge init
```

### `forge build`

Compiles your smart contracts.

```sh
forge build
```

### `forge test`

Runs tests for your smart contracts.

```sh
forge test
```

### `forge create`

Deploys a smart contract to the network.

```sh
forge create <CONTRACT_NAME> --rpc-url <RPC_URL> --private-key <PRIVATE_KEY>
```

### `forge install`

Installs a dependency.

```sh
forge install <DEPENDENCY>
```

### `forge update`

Updates a dependency.

```sh
forge update <DEPENDENCY>
```

### `forge remove`

Removes a dependency.

```sh
forge remove <DEPENDENCY>
```

### `forge config`

Displays or sets configuration options.

```sh
forge config [OPTIONS]
```

### `forge clean`

Cleans the build artifacts and cache.

```sh
forge clean
```

### `forge snapshot`

Creates a gas usage snapshot.

```sh
forge snapshot
```

### `forge flatten`

Flattens the source code of a contract.

```sh
forge flatten <CONTRACT_PATH>
```

### `forge inspect`

Inspects a contract for specific details.

```sh
forge inspect <CONTRACT_NAME> [FIELD]
```

### `forge verify-contract`

Verifies a contract on a blockchain explorer.

```sh
forge verify-contract <CONTRACT_ADDRESS> <CONTRACT_NAME> --chain-id <CHAIN_ID> --verifier <VERIFIER>
```

### `forge upload-selectors`

Uploads function selectors to a blockchain explorer.

```sh
forge upload-selectors <CONTRACT_ADDRESS> --chain-id <CHAIN_ID> --verifier <VERIFIER>
```

### `forge cache`

Manages the cache.

```sh
forge cache [OPTIONS]
```

### `forge completions`

Generates shell completions.

```sh
forge completions [SHELL]
```

### `forge help`

Displays help information.

```sh
forge help [COMMAND]
```

---

## Configuration

Forge can be configured via a `foundry.toml` file. This file allows you to set various options such as the default RPC URL, compiler settings, and more.

### Example `foundry.toml`

```toml
[default]
rpc_url = "https://mainnet.infura.io/v3/YOUR_API_KEY"
private_key = "YOUR_PRIVATE_KEY"

[profile.ci]
rpc_url = "https://rinkeby.infura.io/v3/YOUR_API_KEY"
private_key = "YOUR_PRIVATE_KEY"
```

### Configuration Options

- `rpc_url`: The default RPC URL to use.
- `private_key`: The default private key to use.
- `compiler_version`: The Solidity compiler version to use.
- `optimizer`: Enable or disable the Solidity optimizer.
- `optimizer_runs`: The number of optimizer runs.
- `evm_version`: The EVM version to target.
- `libraries`: A list of libraries to link.
- `remappings`: A list of remappings for the compiler.
- `output_dir`: The directory to output build artifacts.
- `cache_dir`: The directory to use for caching.
- `gas_price`: The default gas price to use.
- `gas_limit`: The default gas limit to use.
- `chain_id`: The default chain ID to use.
- `verbosity`: The verbosity level for logging.

---

## Testing

Forge uses the [DappTools](https://dapp.tools/) testing framework by default. You can write tests in Solidity and run them using the `forge test` command.

### Example Test

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "forge-std/Test.sol";
import "../src/MyContract.sol";

contract MyContractTest is Test {
    MyContract public myContract;

    function setUp() public {
        myContract = new MyContract();
    }

    function testIncrement() public {
        myContract.increment();
        assertEq(myContract.counter(), 1);
    }
}
```

### Running Tests

```sh
forge test
```

---

## Deployment

Forge makes it easy to deploy your smart contracts to various networks. You can specify the network and other parameters using the `forge create` command.

### Example Deployment

```sh
forge create MyContract --rpc-url https://mainnet.infura.io/v3/YOUR_API_KEY --private-key YOUR_PRIVATE_KEY
```

### Verifying Contracts

After deploying a contract, you can verify it on a blockchain explorer using the `forge verify-contract` command.

```sh
forge verify-contract <CONTRACT_ADDRESS> MyContract --chain-id 1 --verifier etherscan
```

---

## Dependencies

Forge supports managing dependencies using the `forge install`, `forge update`, and `forge remove` commands.

### Installing a Dependency

```sh
forge install OpenZeppelin/openzeppelin-contracts
```

### Updating a Dependency

```sh
forge update OpenZeppelin/openzeppelin-contracts
```

### Removing a Dependency

```sh
forge remove OpenZeppelin/openzeppelin-contracts
```

---

## Advanced Usage

### Customizing the Compiler

You can customize the Solidity compiler settings in the `foundry.toml` file.

```toml
[default]
compiler_version = "0.8.4"
optimizer = true
optimizer_runs = 200
evm_version = "berlin"
```

### Using Remappings

Remappings allow you to map import paths to different directories.

```toml
[default]
remappings = [
    "openzeppelin-contracts/=lib/openzeppelin-contracts/",
    "my-contracts/=src/"
]
```

### Managing Cache

Forge uses a cache to speed up builds. You can manage the cache using the `forge cache` command.

```sh
forge cache clear
```

### Generating Shell Completions

Forge can generate shell completions for easier command line usage.

```sh
forge completions bash
```

---

## Troubleshooting

### Common Issues

- **Compiler Errors**: Ensure that your Solidity code is correct and that you are using the correct compiler version.
- **RPC Issues**: Ensure that your RPC URL is correct and that you have sufficient permissions.
- **Deployment Failures**: Ensure that your private key is correct and that you have sufficient funds.

### Getting Help

If you encounter issues, you can use the `forge help` command or consult the [Foundry documentation](https://book.getfoundry.sh/).

```sh
forge help
```

---

## Conclusion

Forge is a powerful tool for managing smart contracts, providing a comprehensive set of commands for testing, building, and deploying your contracts. With its flexible configuration options and support for dependencies, Forge is an essential part of the Foundry toolkit.
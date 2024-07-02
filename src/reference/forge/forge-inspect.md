## forge inspect

### NAME

forge-inspect - 获取智能合约的专门信息

### SYNOPSIS

``forge inspect`` [*options*] *contract_name* *field*

### DESCRIPTION

获取智能合约的专门信息。

要检查的字段（*field*）可以是以下任意一项：

- `abi`
- `b`/`bytes`/`bytecode`
- `deployedBytecode`/`deployed_bytecode`/`deployed-bytecode`/`deployedbytecode`/`deployed`
- `assembly`/`asm`
- `asmOptimized`/`assemblyOptimized`/`assemblyoptimized`/`assembly_optimized`/`asmopt`/`assembly-optimized`/`asmo`/`asm-optimized`/`asmoptimized`/`asm_optimized`
- `methods`/`methodidentifiers`/`methodIdentifiers`/`method_identifiers`/`method-identifiers`/`mi`
- `gasEstimates`/`gas`/`gas_estimates`/`gas-estimates`/`gasestimates`
- `storageLayout`/`storage_layout`/`storage-layout`/`storagelayout`/`storage`
- `devdoc`/`dev-doc`/`devDoc`
- `ir`
- `ir-optimized`/`irOptimized`/`iroptimized`/`iro`/`iropt`
- `metadata`/`meta`
- `userdoc`/`userDoc`/`user-doc`
- `ewasm`/`e-wasm`
- `errors`
- `events`

### OPTIONS

`--pretty`  
&nbsp;&nbsp;&nbsp;&nbsp;如果支持，以漂亮的格式打印选定的字段。

{{#include core-build-options.md}}

{{#include common-options.md}}

### EXAMPLES

1. 检查合约的字节码：
    ```sh
    forge inspect MyContract bytecode
    ```

2. 检查合约的存储布局：
    ```sh
    forge inspect MyContract storage
    ```

3. 以漂亮的格式检查合约的 ABI：
   ```sh
   forge inspect --pretty MyContract abi
   ```

### SEE ALSO

[forge](./forge.md), [forge build](./forge-build.md)
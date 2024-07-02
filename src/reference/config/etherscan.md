```json
{
  "translation": "## Etherscan\n\n与Etherscan相关的配置，例如API密钥。Forge在多个地方使用此配置。\n\n`[etherscan]`部分是将键映射到Etherscan配置表的映射。Etherscan配置表包含以下键：\n\n- `key` (字符串) (**必需**): 给定网络的Etherscan API密钥。此属性的值也可以指向环境变量。\n- `chain`: 此Etherscan配置所针对的链的名称或ID。\n- `url`: Etherscan API URL。\n\n如果配置的键是链名称，则不需要`chain`，否则需要。`url`可以用于显式设置名称不原生支持的链的Etherscan API URL。\n\n使用TOML内联表语法，所有这些都是有效的：\n\n```toml\n[etherscan]\nmainnet = { key = \"${ETHERSCAN_MAINNET_KEY}\" }\nmainnet2 = { key = \"ABCDEFG\", chain = \"mainnet\" }\noptimism = { key = \"1234567\" }\nunknown_chain = { key = \"ABCDEFG\", url = \"<etherscan api url for this chain>\" }\n```"
}
```
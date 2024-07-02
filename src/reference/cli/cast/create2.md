```json
{
  "translation": "# cast create2\n\n使用CREATE2生成一个确定性的合约地址\n\n```bash\n$ cast create2 --help\nUsage: cast create2 [OPTIONS]\n\nOptions:\n  -s, --starts-with <HEX>      合约地址的前缀\n  -e, --ends-with <HEX>        合约地址的后缀\n  -m, --matching <HEX>         地址必须匹配的序列\n  -c, --case-sensitive         区分大小写匹配\n  -d, --deployer <ADDRESS>     合约部署者的地址 [默认: 0x4e59b44847b379578588920ca78fbf26c0b4956c]\n  -i, --init-code <HEX>        要部署的合约的初始代码\n      --init-code-hash <HASH>  要部署的合约的初始代码哈希\n  -j, --jobs <JOBS>            使用的线程数。默认值和上限为逻辑核心数\n      --caller <ADDRESS>       调用者的地址。用于盐的前20个字节\n      --seed <HEX>             随机数生成器的种子，用于初始化盐\n      --no-random              不使用随机值初始化盐，而是使用默认值0\n  -h, --help                   打印帮助信息\n```"
}
```
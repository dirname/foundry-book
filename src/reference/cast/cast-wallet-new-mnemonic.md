## cast new-mnemonic

### NAME

cast-wallet-new-mnemonic - 创建一个包含指定数量单词的新助记词。

### SYNOPSIS

``cast wallet new-mnemonic`` [*options*]

### DESCRIPTION

生成一个随机的 BIP39 助记词短语。

### OPTIONS

#### 新助记词选项

`-w`
`--words`
&nbsp;&nbsp;&nbsp;&nbsp;助记词的单词数量。默认为 12。

`-a`
`--accounts`
&nbsp;&nbsp;&nbsp;&nbsp;要显示的账户数量，这些账户由助记词派生。默认为 1。

### EXAMPLES

1. 创建一个包含 24 个单词的新助记词。
    ```sh
    cast wallet new-mnemonic --words 24
    ```
   
```text
成功生成一个新的助记词。
短语:
decrease where seek crop segment want icon medal sleep social blast provide virus grief pledge soccer stereo trick dry dirt rotate explain into nominee

账户:
- 账户 0:
地址:     0x34644D4eC92ae1832877cE22AD9bA4b00c7397c8
私钥: 0x832a3784d0a130c8a0ce3cc6dfc336a41ca7801a117eac7a3bfaace52e4d239c
```

### SEE ALSO

[cast](./cast.md)
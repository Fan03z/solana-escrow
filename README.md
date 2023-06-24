# Solana-Escrow

使用 Rust 编写 Solana 程序,实现交易托管程序,作为交易双方的可信任第三方

其实已经有 anchor 框架将 Solana 的程序借口抽象出来了,一般在开发的时候也不会使用原生的 Solana 程序,但实现一下对理解 Solana 还是很有帮助的

Solana 和 Ethereum 有些差别:

## 账户

Solana 内的账户不管合约账户还是普通账户可以说是都是一种账户来的,看源码定义的话,其实就是在 account_info 下的其中 Executable 变量来区分的。
Solana 内的合约一般来说也是不存储状态数据的,只负责逻辑实现。状态数据都是存储在普通账户上,在调用合约时再传进合约中。这点和以太坊下的合约就不一样了。也正是因为如此,Solana 下的合约要更新的话是比较方便的,毕竟合约内没数据。

## PDA (Program Derived Address)

通俗点理解的话就是让程序调用别的账户用的,因为合约账户只负责实现逻辑,跨程序的调用的就实现了合约程序的复用。因此 PDA 也是实现 CPI 的基础。PDA 没有是没有私钥只有公钥的地址。粗略看实现来说的话,就是通过在调用椭圆曲线时,给定一个 seed,使得得到结果偏离曲线,从而在剔除私钥的同时,又因为根据原程序数据和加入的 seed 保证了一对一。用来绑定账户的,一般而言一合约程序账户也就对应着一个 PDA。

## CPI (Cross-Program Invocation)

原生的 Solana 程序的 CPI 在实现手段上有两个方法:

```rust
// 1. invoke() 不需要PDA签名
invoke(
  &some_instruction, // 调用的指令
  &[account_one.clone(), account_two.clone()], // 指令需要传入的账户参数
)?;

// 2. invoke_signed
invoke_signed(
  &some_instruction,                   // 调用指令
  &[account_one.clone(), pda.clone()], // 指令需要传入的账户参数,最后一个是签名需要的 PDA
  &[signers_seeds],                    // 设定 PDA 时绑定的 seed
)?;
```

注: 一个交易 (TX) 可以包含多个指令 (IX) (一般也是包含多个)

部署 Solana 程序步骤:

1. `cargo build-bpf` 得到 /target 文件夹
2. `solana program deploy ./target/deploy/solana_escrow.so` 部署程序,deploy 后跟着 /target 文件夹下 .so 文件路径

调用程序的 /scripts 没有多改,看着明白在做什么就好了,里面挺多都是比较久的库和弃用的方法

[学习博客](https://paulx.dev/blog/2021/01/14/programming-on-solana-an-introduction/?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDQ0ODk5NjIsImciOiI4QXcyeDJNQTJrUVVYdnYwIiwiaWF0IjoxNjQ0NDg5NjYyLCJ1c2VySWQiOi0xNzQ4NzYxNTMyfQ.CtyE1BpTwlKzCmRd-9GUk_Al4ksvc0vuihwPKkWgVBQ#trying-out-the-program-understanding-alice-s-transaction)

[Solana bootcamp](https://www.bilibili.com/video/BV1jY411K7Xj/)

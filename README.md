# Solana-Escrow

使用 Rust 编写 Solana 程序,实现交易托管程序

其实已经有 anchor 框架将 Solana 的程序借口抽象出来了,一般在开发的时候也不会使用原生的 Solana 程序,但实现一下对理解 Solana 还是很有帮助的

Solana 和 Ethereum 有些差别:

- **账户**
- **PDA**
- **CPI**

[学习博客](https://paulx.dev/blog/2021/01/14/programming-on-solana-an-introduction/?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDQ0ODk5NjIsImciOiI4QXcyeDJNQTJrUVVYdnYwIiwiaWF0IjoxNjQ0NDg5NjYyLCJ1c2VySWQiOi0xNzQ4NzYxNTMyfQ.CtyE1BpTwlKzCmRd-9GUk_Al4ksvc0vuihwPKkWgVBQ#trying-out-the-program-understanding-alice-s-transaction)

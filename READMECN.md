# Rust 程序设计语言

[English](README.md) | **简体中文**

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

本仓库包含《Rust 程序设计语言》一书的源码。

纸质版可在 No Starch Press 获取：<https://nostarch.com/rust-programming-language-2nd-edition>

在线阅读：
- stable: <https://doc.rust-lang.org/stable/book/>
- beta: <https://doc.rust-lang.org/beta/book/>
- nightly: <https://doc.rust-lang.org/nightly/book/>

代码示例打包下载：<https://github.com/rust-lang/book/releases>

## 环境要求

构建本书需要 [mdBook](https://github.com/rust-lang/mdBook)，建议与 rust-lang/rust 所使用版本一致：
<https://github.com/rust-lang/rust/blob/HEAD/src/tools/rustbook/Cargo.toml>

安装命令：

```bash
cargo install mdbook --locked --version <version_num>
```

## 构建

```bash
mdbook build
```

构建产物在 `book/` 目录。

## 测试

```bash
cd packages/trpl
mdbook test --library-path packages/trpl/target/debug/deps
```

## 贡献

欢迎贡献！请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

翻译相关议题：
- <https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations>
- <https://github.com/rust-lang/mdBook/issues/5>

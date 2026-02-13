## 附录 D：有用的开发工具

在本附录中，我们讨论 Rust 中的一些有用的开发工具
项目提供。我们将研究自动格式化、快速应用方法
警告修复、linter 以及与 IDE 集成。

### 使用`rustfmt`自动格式化

`rustfmt` 工具根据社区代码风格重新格式化您的代码。
许多协作项目使用 `rustfmt` 来防止关于哪个项目的争论
编写 Rust 时使用的样式：每个人都使用该工具格式化他们的代码。

Rust 安装默认包含`rustfmt`，所以你应该已经有了
在您的系统上编写`rustfmt` 和`cargo-fmt` 程序。这两个命令是
与`rustc` 和`cargo` 类似，`rustfmt` 允许更细粒度的控制
`cargo-fmt` 理解使用 Cargo 的项目的约定。格式化
任何 Cargo 项目，输入以下内容：

```console
$ cargo fmt
```

运行此命令会重新格式化当前包中的所有 Rust 代码。这
应该只改变代码风格，而不是代码语义。欲了解更多信息
关于`rustfmt`，请参阅[其文档][rustfmt]。

### 使用 `rustfix` 修复您的代码

`rustfix` 工具包含在 Rust 安装中，可以自动
修复编译器警告，有一个明确的方法来纠正问题
可能是你想要的。您以前可能见过编译器警告。为了
例如，考虑这段代码：

<span class="filename">文件名：src/main.rs</span>

```rust
fn main() {
    let mut x = 42;
    println!("{x}");
}
```

在这里，我们将变量 `x` 定义为可变的，但我们从未真正改变
它。 Rust 警告我们：

```console
$ cargo build
   Compiling myprogram v0.1.0 (file:///projects/myprogram)
warning: variable does not need to be mutable
 --> src/main.rs:2:9
  |
2 |     let mut x = 0;
  |         ----^
  |         |
  |         help: remove this `mut`
  |
  = note: `#[warn(unused_mut)]` on by default
```

该警告建议我们删除 `mut` 关键字。我们可以自动
通过运行命令 `cargo，使用 `rustfix` 工具应用该建议
修复`：

```console
$ cargo fix
    Checking myprogram v0.1.0 (file:///projects/myprogram)
      Fixing src/main.rs (1 fix)
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

当我们再次查看 _src/main.rs_ 时，我们会看到 `cargo fix` 已更改
代码：

<span class="filename">文件名：src/main.rs</span>

```rust
fn main() {
    let x = 42;
    println!("{x}");
}
```

变量 `x` 现在是不可变的，并且警告不再出现。

您还可以使用`cargo fix`命令在之间转换代码
不同的 Rust 版本。版本包含在[附录 E][版本]<!--
忽略-->。

### 使用 Clippy 获得更多 lints

Clippy 工具是一个 lint 集合，用于分析您的代码，以便您可以
发现常见错误并改进您的 Rust 代码。 Clippy 包含在
标准 Rust 安装。

要在任何 Cargo 项目上运行 Clippy 的 lint，请输入以下内容：

```console
$ cargo clippy
```

例如，假设您编写了一个使用近似值的程序
数学常数，例如 pi，如以下程序所示：

<列表文件名=“src/main.rs”>

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

</清单>

在此项目上运行 `cargo clippy` 会导致以下错误：

```text
error: approximate value of `f{32, 64}::consts::PI` found
 --> src/main.rs:2:13
  |
2 |     let x = 3.1415;
  |             ^^^^^^
  |
  = note: `#[deny(clippy::approx_constant)]` on by default
  = help: consider using the constant directly
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#approx_constant
```

这个错误让你知道 Rust 已经有一个更精确的 `PI` 常量
定义，并且如果您使用常量，您的程序会更正确
反而。然后，您可以更改代码以使用 `PI` 常量。

以下代码不会导致 Clippy 发出任何错误或警告：

<列表文件名=“src/main.rs”>

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

</清单>

有关 Clippy 的更多信息，请参阅[其文档][clippy]。

### 使用 `rust-analyzer` 进行 IDE 集成

为了帮助 IDE 集成，Rust 社区建议使用
[`rust-analyzer`][rust-analyzer]<!-- 忽略 -->。这个工具是一套
以编译器为中心的实用程序，使用 [语言服务器协议][lsp]<!--
忽略 -->，这是 IDE 和编程语言的规范
互相沟通。不同的客户端可以使用`rust-analyzer`，比如
[适用于 Visual Studio Code 的 Rust 分析器插件][vscode]。

访问`rust-analyzer`项目的[主页][rust-analyzer]<!-- 忽略-->
有关安装说明，然后在您的计算机中安装语言服务器支持
特定的IDE。您的 IDE 将获得自动完成、跳转到等功能
定义和内联错误。

[rustfmt]: https://github.com/rust-lang/rustfmt
[editions]: appendix-05-editions.md
[clippy]: https://github.com/rust-lang/rust-clippy
[rust-analyzer]: https://rust-analyzer.github.io
[lsp]: http://langserver.org/
[vscode]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

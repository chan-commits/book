## 附录 A：关键词

以下列表包含为当前或将来保留的关键字
由 Rust 语言使用。因此，它们不能用作标识符（除了
作为原始标识符，正如我们在[“原始
标识符”][原始标识符]<!-- 忽略 --> 部分）。_标识符_ 是名称
函数、变量、参数、结构字段、模块、包、常量，
宏、静态值、属性、类型、特征或生命周期。

[raw-identifiers]: #raw-identifiers

### 当前使用的关键字

以下是当前使用的关键字及其功能的列表
描述的。

- **`as`**：执行原始转换，消除特定特征的歧义
  包含项目，或重命名 `use` 语句中的项目。
- **`async`**：返回一个`Future`，而不是阻塞当前线程。
- **`await`**：暂停执行，直到`Future`的结果准备好。
- **`break`**：立即退出循环。
- **`const`**：定义常量项或常量原始指针。
- **`continue`**：继续下一个循环迭代。
- **`crate`**：在模块路径中，指 crate 根。
- **`dyn`**：动态分派到特征对象。
- **`else`**：`if` 和`if let` 控制流构造的后备。
- **`enum`**：定义一个枚举。
- **`extern`**：链接外部函数或变量。
- **`false`**：布尔假文字。
- **`fn`**：定义函数或函数指针类型。
- **`for`**：循环迭代器中的项目，实现特征，或指定
  排名较高的生命周期。
- **`if`**：根据条件表达式的结果进行分支。
- **`impl`**：实现固有或特征功能。
- **`in`**：`for` 循环语法的一部分。
- **`let`**：绑定变量。
- **`loop`**：无条件循环。
- **`match`**：将值与模式匹配。
- **`mod`**：定义一个模块。
- **`move`**：使闭包拥有其所有捕获的所有权。
- **`mut`**：表示引用、原始指针或模式绑定中的可变性。
- **`pub`**：表示结构体字段、`impl` 块中的公共可见性，或
  模块。
- **`ref`**：通过引用绑定。
- **`return`**：从函数返回。
- **`Self`**：我们正在定义或实现的类型的类型别名。
- **`self`**：方法主题或当前模块。
- **`static`**：全局变量或持续整个程序的生命周期
  执行。
- **`struct`**：定义一个结构体。
- **`super`**：当前模块的父模块。
- **`trait`**：定义一个特征。
- **`true`**：布尔真值。
- **`type`**：定义类型别名或关联类型。
- **`union`**：定义一个[union][union]<!-- 忽略-->;仅当
  在联合声明中使用。
- **`unsafe`**：表示不安全的代码、函数、特征或实现。
- **`use`**：将符号纳入范围。
- **`where`**：表示约束类型的子句。
- **`while`**：根据表达式的结果有条件地循环。

[union]: ../reference/items/unions.html

### 保留供将来使用的关键字

以下关键字尚无任何功能，但已被保留
Rust 未来的潜在用途：

- `abstract`
- `become`
- `box`
- `do`
- `final`
- `gen`
- `macro`
- `override`
- `priv`
- `try`
- `typeof`
- `unsized`
- `virtual`
- `yield`

### 原始标识符

_原始标识符_是允许您在不使用关键字的地方使用关键字的语法
通常是允许的。您可以通过在关键字前添加 `r#` 来使用原始标识符。

例如，`match` 是一个关键字。如果你尝试编译以下函数
使用 `match` 作为其名称：

<span class="filename">文件名：src/main.rs</span>

```rust,ignore,does_not_compile
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```

你会得到这个错误：

```text
error: expected identifier, found keyword `match`
 --> src/main.rs:4:4
  |
4 | fn match(needle: &str, haystack: &str) -> bool {
  |    ^^^^^ expected identifier, found keyword
```

错误表明不能使用关键字`match`作为函数
标识符。要使用`match`作为函数名称，您需要使用原始
标识符语法，如下所示：

<span class="filename">文件名：src/main.rs</span>

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

这段代码将编译，没有任何错误。请注意函数上的 `r#` 前缀
定义中的名称以及 `main` 中调用该函数的位置。

原始标识符允许您使用您选择的任何单词作为标识符，即使
该词恰好是保留关键字。这给了我们更多的选择自由
标识符名称，以及让我们与用
这些词不是关键字的语言。此外，原始标识符允许
您可以使用与您的 crate 使用的 Rust 版本不同的库编写的库。
例如，`try`在2015年版本中不是关键字，但在2018年、2021年、
和 2024 年版本。如果您依赖于使用 2015 编写的库
版本并具有 `try` 函数，您需要使用原始标识符语法，
在本例中为`r#try`，用于从后续版本的代码中调用该函数。
有关版本的更多信息，请参阅[附录 E][附录-e]<!--ignore-->。

[appendix-e]: appendix-05-editions.html

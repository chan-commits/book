## 附录 C：可衍生特征

在本书的不同地方，我们讨论了 `derive` 属性，它
您可以应用于结构或枚举定义。 `derive` 属性生成
将使用其自己的默认实现来实现特征的代码
使用 `derive` 语法注释的类型。

在本附录中，我们提供了标准中所有特征的参考
您可以与 `derive` 一起使用的库。每个部分涵盖：

- 派生此特征的运算符和方法将启用什么
- `derive` 提供的特征的实现做了什么
- 实现该特征对类型意味着什么
- 允许或不允许您实施该特征的条件
- 需要该特征的操作示例

如果您想要与 `derive` 属性提供的行为不同的行为，
查阅[标准库文档](../std/index.html)<!-- 忽略-->
对于每个特征，了解如何手动实现它们的详细信息。

此处列出的特征是标准库定义的唯一特征
可以使用 `derive` 在您的类型上实现。中定义的其他特征
标准库没有合理的默认行为，所以这取决于你
以对您想要实现的目标有意义的方式实施它们。

无法派生的特征的一个例子是`Display`，它处理
为最终用户格式化。您应该始终考虑采取适当的方式
向最终用户显示类型。最终用户应该属于该类型的哪些部分
允许看吗？他们会发现哪些部分相关？数据是什么格式
与他们最相关？ Rust 编译器没有这种洞察力，所以
它无法为您提供适当的默认行为。

本附录中提供的可衍生特征列表并不全面：
图书馆可以为自己的特征实现`derive`，列出
您可以使用 `derive` 真正开放式的特征。实施`derive`
涉及使用程序宏，该宏包含在 [“Custom `derive`
宏”][custom-derive-macros]<!-- 忽略 --> 第 20 章中的部分。

### `Debug` 用于编程器输出

`Debug` 特征可以在格式字符串中启用调试格式，您可以使用它
通过在`{}` 占位符中添加`:?` 来表示。

`Debug` 特征允许您打印类型的实例以进行调试
目的，以便您和其他使用您的类型的程序员可以检查实例
在程序执行的特定时刻。

`Debug` 特征是必需的，例如，在使用 `assert_eq!` 时
宏。该宏打印作为参数给出的实例的值，如果
相等断言失败，以便程序员可以了解为什么这两个实例
不平等。

### `PartialEq` 和 `Eq` 用于相等比较

`PartialEq` 特征允许您比较类型的实例以检查
相等并允许使用 `==` 和 `!=` 运算符。

派生`PartialEq` 实现`eq` 方法。当 `PartialEq` 派生于
结构体中，仅当 _all_ 字段相等时两个实例才相等，并且
如果_any_字段不相等，则实例不相等。当从枚举派生时，
每个变体都等于其自身，但不等于其他变体。

`PartialEq` 特征是必需的，例如，使用
`assert_eq!` 宏，需要能够比较类型的两个实例
为了平等。

`Eq` 特征没有方法。其目的是表明对于每个值
带注释的类型，值等于其自身。 `Eq` 特征只能是
适用于也实现 `PartialEq` 的类型，尽管并非所有类型
实现`PartialEq`可以实现`Eq`。浮点就是一个例子
数字类型：浮点数的实现指出两个
非数字 (`NaN`) 值的实例彼此不相等。

需要 `Eq` 的一个示例是 `HashMap<K, V>` 中的键，以便
`HashMap<K, V>` 可以判断两个密钥是否相同。

### `PartialOrd` 和 `Ord` 用于订购比较

`PartialOrd` 特征允许您比较类型的实例以进行排序
目的。实现`PartialOrd`的类型可以与`<`、`>`、
`<=` 和 `>=` 运算符。您只能将 `PartialOrd` 特征应用于类型
也实现`PartialEq`。

派生 `PartialOrd` 实现 `partial_cmp` 方法，该方法返回一个
当给定的值不产生一个时，`Option<Ordering>`将是`None`
订购。一个不产生排序的值的示例，即使
该类型的大多数值都可以进行比较，即 `NaN` 浮点值。
使用任何浮点数和`NaN`调用`partial_cmp`
浮点值将返回`None`。

当在结构上派生时，`PartialOrd` 通过比较两个实例
每个字段中的值按照字段在结构中出现的顺序排列
定义。当从枚举派生时，前面声明的枚举的变体
枚举定义被认为小于后面列出的变体。

`PartialOrd` 特征是必需的，例如，对于 `gen_range` 方法
来自 `rand` 板条箱，它生成指定范围内的随机值
范围表达式。

`Ord` 特征让您知道对于带注释的任意两个值
类型，将存在有效的排序。 `Ord` 特征实现了 `cmp` 方法，
它返回`Ordering`而不是`Option<Ordering>`，因为有效
订购将永远是可能的。您只能将 `Ord` 特征应用于类型
还实现`PartialOrd`和`Eq`（`Eq`需要`PartialEq`）。什么时候
在结构和枚举上派生，`cmp` 的行为方式与派生的相同
`partial_cmp` 的实现与 `PartialOrd` 一起实现。

需要 `Ord` 的一个示例是在 `BTreeSet<T>` 中存储值时，
根据值的排序顺序存储数据的数据结构。

### `Clone` 和 `Copy` 用于重复值

`Clone` 特征允许您显式创建值的深层副本，并且
复制过程可能涉及运行任意代码和复制堆
数据。请参阅[“变量和数据与
克隆”][变量和数据与克隆交互]<!-- 忽略 --> 部分
有关 `Clone` 的更多信息，请参阅第 4 章。

派生 `Clone` 实现 `clone` 方法，该方法在为
整个类型，在类型的每个部分上调用`clone`。这意味着所有
类型中的字段或值还必须实现 `Clone` 才能派生 `Clone`。

需要 `Clone` 的示例是在调用 `to_vec` 方法时
片。切片不拥有它所包含的类型实例，但拥有向量
从 `to_vec` 返回将需要拥有其实例，因此 `to_vec` 调用
每个项目`clone`。因此，存储在切片中的类型必须实现`Clone`。

`Copy` 特征允许您通过仅复制存储在的位来复制值
堆栈；不需要任意代码。请参阅[“仅堆栈数据：
Copy”][stack-only-data-copy]<!--ignore--> 第 4 章中的部分了解更多信息
关于`Copy`的信息。

`Copy` 特征没有定义任何方法来阻止程序员
重载这些方法并违反了没有任意代码的假设
正在运行。这样，所有程序员都可以假设复制一个值将是
非常快。

您可以在其部分全部实现 `Copy` 的任何类型上派生 `Copy`。一种类型
实现`Copy` 也必须实现`Clone`，因为实现了的类型
`Copy` 有一个简单的 `Clone` 实现，它执行与
`Copy`。

`Copy` 特征很少需要；实现 `Copy` 的类型有
可用的优化，这意味着您不必调用`clone`，这使得
代码更加简洁。

使用 `Copy` 可以实现的一切，您也可以使用 `Clone` 完成，但是
代码可能会更慢或者必须在某些地方使用`clone`。

### `Hash` 用于将值映射到固定大小的值

`Hash` 特征允许您获取任意大小的类型的实例，并且
使用哈希函数将该实例映射到固定大小的值。推导
`Hash` 实现`hash` 方法。 `hash` 的派生实现
方法组合了对类型的每个部分调用 `hash` 的结果，
这意味着所有字段或值还必须实现 `Hash` 才能派生 `Hash`。

需要 `Hash` 的一个示例是将密钥存储在 `HashMap<K, V>` 中
有效地存储数据。

### 默认值`Default`

`Default` 特征允许您为类型创建默认值。推导
`Default` 实现`default` 功能。的派生实现
`default` 函数在类型的每个部分上调用 `default` 函数，
意味着类型中的所有字段或值也必须实现`Default`
导出`Default`。

`Default::default` 函数通常与结构体结合使用
更新语法在[“Creating Instances from Other Instances with
结构更新
语法”][使用结构更新语法从其他实例创建实例]<!--
第 5 章中的忽略 --> 部分。您可以自定义结构体的一些字段并
然后使用以下命令为其余字段设置并使用默认值
`..Default::default()`。

当您使用 `unwrap_or_default` 方法时，需要 `Default` 特征
例如，`Option<T>` 实例。如果`Option<T>`是`None`，则该方法
`unwrap_or_default` 将返回 `Default::default` 类型的结果
`T` 存储在`Option<T>` 中。

[creating-instances-from-other-instances-with-struct-update-syntax]: ch05-01-defining-structs.html#creating-instances-from-other-instances-with-struct-update-syntax
[stack-only-data-copy]: ch04-01-what-is-ownership.html#stack-only-data-copy
[variables-and-data-interacting-with-clone]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-clone
[custom-derive-macros]: ch20-05-macros.html#custom-derive-macros

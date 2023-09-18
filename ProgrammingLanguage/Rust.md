# Rust methods and Traits

# 在rust中常见编程概念

## 变量和可变性

在Rust中，可以运用 `let` 语句去定义变量，变量是默认不可变（immutable）的。

> 在尝试改变预设为不可变的值时，产生编译时错误是很重要的，因为这种情况可能导致 bug。如果一部分代码假设一个值永远也不会改变，而另一部分代码改变了这个值，第一部分代码就有可能以不可预料的方式运行。不得不承认这种 bug 的起因难以跟踪，尤其是第二部分代码只是 **有时** 会改变值。

Rust以这种方式保证了变量内容的安全性，当有另一部分代码尝试修改某些预设不变的值时，将会无法通过编译，从根本上杜绝了上述bug的产生。

变量的可变性是一个非常重要的特性，Rust提供了 `mut` 关键词，能显性声明一个变量具有可变性。`mut` 也向读者表明了其他代码将会改变这个变量值的意图。

示例代码
``` rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

### 常量

*常量(constants)* 是绑定到一个名称的不允许改变的值，虽然很类似于不可变变量，但是仍有不同。

1. 不允许对常量使用 `mut` 关键字，这保证了常量的永不可变特性。
2. 声明常量使用的是 `const` 而不是 `let`，并且***必须***注明常量的数据类型。
3. 常量只能设置为常量表达式，即此表达式必须在运行前就能计算出结果。

示例代码
``` rust
const THREE_HOURS_IN_SECONDS : u32 = 60 * 60 * 3;
```

注意，Rust对常量的命名约定是在单词间使用全大写和下划线连接。在定义常量时使用常量表达式而非常量本身，以此来增强代码的可读性。如上例，用60 * 60 * 3来定义而非10800这一结果值。

>在声明它的作用域中，常量在整个程序的生命周期都有效，此属性使得常量可以作为多处代码使用的全局范围的值，例如一个游戏中所有玩家可以获取的最高分或者光速。
>
>将遍布于应用程序中的硬编码值声明为常量，能帮助后来的代码维护人员了解值的意图。如果将来需要修改硬编码值，也只需修改汇聚于一处的硬编码值。

### 隐藏(Shadowing)

Rust的特性——隐藏(Shadowing)可以让我们可以定义一个与之前变量同名的新变量，就如同第一个变量被后一个第二个新变量***隐藏***了。这以为着在新变量被隐藏前或声明其的作用域结束前，编译器都看到的是第二个变量，任何使用该变量名的行为都会视为在使用第二个变量。

可以使用相同变量名称来隐藏第一个变量，或者重复使用 `let` 关键词来多次隐藏。

示例代码
``` rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

```

运行结果
```
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/variables`
The value of x in the inner scope is: 12
The value of x is: 6
```

注意，隐藏与mut是有区别的。

- 当不小心对变量重新赋值时，若没有使用 `let` 关键字，则会导致编译错误。这个特性使得在写程序时，需要更加谨慎地考虑到变量值的安全性。
	通过使用 `let` 关键字，我们可以用这个值进行一些计算，但是在计算之后变量仍是不可变的。

- 使用 `let` 隐藏一个变量时，实际上是创造了一个新的变量，即我们可以改变这个变量名所绑定的变量类型，而 `mut` 关键字无法改变一个变量的类型。
	例如，假设程序请求用户输入空格字符来说明希望在文本之间显示多少个空格，接下来我们想将输入存储成数字（多少个空格）：
	``` rust
	fn main() {
	    let spaces = "   ";
	    let spaces = spaces.len();
	}
	```
	第一个 `spaces` 是字符串类型，第二个 `spaces` 则通过 `let` 用数字类型将字符串类型隐藏了起来。 隐藏使我们不必使用不同的名字，如 `spaces_str` 或 `spaces_num`，相反只需要使用 `spaces` 这一简单的名字即可。


## 数据类型

Rust是一个静态类型 *(statically typed)* 语言，即在编译时编译器需要知道所有的变量的类型。根据值及其使用方法，rust的编译器常常能推断出其数据类型。但当有多种可能的数据类型时，编译器要求必须要增加类型注解。例如，上章使用 `parse` 将 `String` 转换为数字时：

``` rust
let guess: u32 = "42".parse().expect("Not a number!");
```

数据类型有两种子集，标量 (scalar) 和 复合 (compound)。

### 标量

标量类型代表一个单独的值。

Rust有四种基本的标量类型：整型、浮点型、布尔类型和字符型。

#### 整型

整型的定义与C++类似。

下表展示了Rust内建的整型类型。

| 长度 | 有符号 | 无符号|
| ----- | ----- | -----|
| 8-bit | i8 | u8 |
| 16-bit| i16| u16|
| 32-bit| i32| u32|
| 64-bit| i64| u64|
| 128-bit| i128| u128|
| arch | isize| usize|

有符号数以补码的形式储存在内存中。

注意，`isize` 和 `usize` 类型的长度依赖于运行程序的计算机架构：64位架构上它们是64位的，32位架构上则是32位的。

下表展示了Rust中可用的的整型字面值。

| 数字字面值 | 例子 |
| ----------- | ----|
| Decimal | 9_8222 |
| Hex | 0xff |
| Octal | 0o77 |
| Binary | 0b1111_0000 |
| Byte(单字节字符)(仅限于 `u8`) | b'A' |

注意数字字面值可以使用类型后缀来指定类型。 为了增加程序的可读性，rust允许用 `_` 作为分割符方便读数。

数字类型默认是 `i32` ， `isize` 和 `usize` 主要作为某些集合的索引。

> 整型溢出
> 比方说有一个 `u8` ，它可以存放从零到 `255` 的值。那么当你将其修改为 `256` 时会发生什么呢？这被称为 “整型溢出”（“integer overflow” ），这会导致以下两种行为之一的发生。当在 debug 模式编译时，Rust 检查这类问题并使程序 _panic_。
>
>使用 `--release` flag 在 release 模式中构建时，Rust **不会**检测会导致 panic 的整型溢出。相反发生整型溢出时，Rust 会进行一种被称为二进制补码 wrapping（_two’s complement wrapping_）的操作。简而言之，比此类型能容纳最大值还大的值会回绕到最小值，值 `256` 变成 `0`，值 `257` 变成 `1`，依此类推。程序不会 panic，不过变量可能也不会是你所期望的值。依赖整型溢出 wrapping 的行为被认为是一种错误。
>
>为了显式地处理溢出的可能性，可以使用这几类标准库提供的原始数字类型方法：
> -   所有模式下都可以使用 `wrapping_*` 方法进行 wrapping，如 `wrapping_add`
> -   如果 `checked_*` 方法出现溢出，则返回 `None`值
> -   用 `overflowing_*` 方法返回值和一个布尔值，表示是否出现溢出
> -   用 `saturating_*` 方法在值的最小值或最大值处进行饱和处理

#### 浮点型

Rust有两种不同的浮点数 *(floating-point numbers)* 类型，它们是带小数点的数字，即 `f32` 和 `f64`。
默认类型为 `f64`。浮点数采用 IEEE - 754 标准表示，所有浮点型都是有符号的。

e.g.

``` rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

### 数值运算

Rust中的数字类型都支持基本的数学运算：加减、乘除、取余。整数除法向下取整。

e.g.
``` rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // 结果为 -1

    // remainder
    let remainder = 43 % 5;
}
```

这些语句中的每个表达式使用了一个数学运算符并计算出了一个值，然后绑定给一个变量。

### 布尔型

Rust中的布尔类型 `bool` 有两个可能的值：`true` 和 `false` 。

e.g.
``` rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

使用 `bool` 的主要场景是条件表达式，在控制流中有广泛的作用。

### 字符类型

`char` 是rust中最原生的字符类型。

e.g.
``` rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

注意，用单引号声明 `char` 字面量，而用用双引号声明字符串字面量。

Rust的 `char` 类型是 ***4字节*** ，代表一个 `Unicode` 标量值。

>在 Rust 中，带变音符号的字母（Accented letters），中文、日文、韩文等字符，emoji（绘文字）以及零长度的空白字符都是有效的 `char` 值。Unicode 标量值包含从 `U+0000` 到 `U+D7FF` 和 `U+E000` 到 `U+10FFFF` 在内的值。不过，“字符” 并不是一个 Unicode 中的概念，所以人直觉上的 “字符” 可能与 Rust 中的 `char` 并不符合。

### 复合类型

有两种复合类型，元组 `tuple` 和 数组 `array`。

#### tuple 元组

`tuple` 是能将多个其他类型的值组合进一个复合类型的主要方式。 `tuple` 的长度是固定的：一旦声明无法修改。

**1. 可以用圆括号加逗号来声明一个 `tuple`，而且 `tuple` 中每一个位置都有一个类型，不同值的类型可以不相同。**

e.g.
``` rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

因为每个 `tuple` 都被视为一个单独的复合类型，所以上例的 `tup` 变量绑定到整个 `tuple` 上。

**1. 为了从 `tuple` 中获取单个值，可以使用 *pattern matching* 的方式来解构 *(destructure)* `tuple`。**

e.g.
``` rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

程序首先创建了一个 `tuple` 并绑定在了 `tup` 变量上，接着又使用一个模式 *pattern* 和 `let` 将 `tup` 分成了三个不同的变量 `x`, `y`, `z`。因为它将一个 `tuple` 拆分成了三个元素，就称之为 ***解构（destructuring）*** 。

**3. 访问 `tuple` 中的值也可以通过点号( `.` )后跟值的索引来直接访问。**

e.g.
``` rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

这段程序创造了一个 `tuple` ，`x`，然后用其各自的索引访问 `tuple` 中的每个元素。可见 `tuple` 的第一个元素的索引值是 *0* 。

不带任何值的元组有个特殊的名称，叫做 **单元（unit）** 元组 `unit tuple`。==这种值以及对应的类型都写作 `()`，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值。==

#### array 数组

另一个包含多个值的方法是数组 ***(array)***。与 `tuple` 不同，`array` 中的每一个元素的数据类型都必须相同。==注意Rust中的 `array` 长度是固定的。==

**1. 声明数组用方括号，值用逗号分隔。**

e.g.
``` rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

>当你想要在栈（stack）而不是在堆（heap）上为数据分配空间，或者是想要确保总是有固定数量的元素时，数组非常有用。

但是数组并不如 vector 类型灵活。vector 类型是标准库提供的一个 **允许** 增长和缩小长度的类似数组的集合类型。当不确定是应该使用数组还是 vector 的时候，那么很可能应该使用 vector。

然而，当确定了元素个数不会改变时，数组会更有用。

例如，当在一个程序中使用月份名字时，更应该趋向于使用 `array` 而非 `vector`，因为确定只有12个元素。

``` rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

**2. 还可以这样声明 `array` ：在方括号中包含每个元素的类型，后跟分号，再后跟数组元素的数量。**

``` rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

这里，`i32` 明确了数组中元素的类型，分号后的 `5` 表明该数组包含五个元素。

**3. 通过在方括号中指定初始值加分号再加元素个数的方式来创建一个每个元素都为相同值的数组：**

``` rust
let a = [3; 5];
```

变量名为 `a` 的数组将包含 `5` 个元素，这些元素的值最初都将被设置为 `3`。这种写法与 `let a = [3, 3, 3, 3, 3];` 效果相同，但更简洁。

##### 访问数组元素

数组是可以在 `stack` 上分配的已知固定大小的单个内存块。

**1. 可以使用索引来访问数组的元素。**

``` rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

在这个例子中，`first` 变量绑定到了 `a[0]` 上，其值为 `1`，`second` 变量同理。

##### 无效的数组元素访问

当程序尝试访问一个超出数组原本长度的无效索引时，将会出现这样的**运行时**错误的报错信息：

```
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度。如果索引超出了数组长度，Rust 会 _panic_，这是 Rust 术语，它用于程序因为错误而退出的情况。这种检查必须在运行时进行。

### Slice

`slice` 类型是连续序列的动态视图。这里的连续指的是每个元素与其相邻的元素的距离是相同的。

`slice` 本质上是内存块的视图，其用指针和长度来表示。

``` rust
// slicing a vec
let vec = vec![1, 2, 3];
let int_slice = &vec[..];

// coercing an array to a slice
let str_slice : &[&str] = &["one", "two", "three"];
```

`slice` 是可变 *(mutable)* 的或是共用 *(shared)* 的。 `shared slice` 的类型是 `&[T]`, 而 `mutable slice` 的类型是 `&mut [T]`， 这里的 `T` 指的是元素类型。

例如，你可以通过 `mutable slice` 来改变其指向的内存块。

``` rust
let mut x = [1, 2, 3];

```






## 所有权 Ownership

Rust通过所有权系统来管理内存。

### 所有权规则

>   1.  Rust 中的每一个值都有一个 **所有者**（_owner_）。
>   2.  值在任一时刻有且只有一个所有者。
>   3.  当所有者（变量）离开作用域，这个值将被丢弃。

### 变量作用域

**作用域(scope)** 是一个项(item) 在程序中有效的范围。

e.g.
假设有一个绑定到了字符串字面值的变量 `s` ，这个字符串字面值时硬编码进程序代码中的。这个变量从声明开始的点到 **作用域** 结束的点为止都是有效的。

``` rust
{
	let s = "hello";   // 声明s
	
	// 使用s
}  // 作用域结束，s失效
```

### 内存与分配

在Rust中，字符串字面值时不可变的。遇到需要进行字符串修改的情况时，Rust提供了一个 `String` 的数据类型，他是可以可变的。两者的差别就在它们对内存的处理上。

字符串字面值在编译时我们就已知了它的内容，因为它不可变，所以其文本被直接硬编码到最终的可执行文件中。这使得字符串字面值快速且高效。

对于 `String` 类型，为了支持一个实时可发生变化的文本片段。Rust需要在 `Heap` 上分配一块在编译时未知大小的内存来存放内容，这意味着：

-   必须在运行时向内存分配器（memory allocator）请求内存。
-   需要一个当我们处理完 `String` 时将内存返回给分配器的方法。

第一个部分在初始化 `String` 时，相对应的 `method` 实现了请求 `memory allocate` 的部分，在这里 Rust与其他常见编程语言相同。

第二部分实现起来各个语言间就有区别了，`C` 语言这种没有 **垃圾回收** *（garbage collector, GC）* 机制的语言要求编程时就识别出不需要的内存并显性地进行 *释放*。 而 `Java` 这些实现了 *GC* 的语言， *GC* 就会记录并清除不需要的内存。

Rust采用了一个不同的策略：内存在拥有它的变量在离开作用域后就被自动释放。

e.g.
``` rust
{
	let s = String::from("hello");  // s声明
	
	// 使用s
}  // s离开了作用域，内存被释放，从此s无效
```

这是一个将 `String` 需要的内存返回给分配器的很自然的位置：当 `s` 离开作用域的时候。当变量离开作用域，Rust 为我们调用一个特殊的函数。这个函数叫做 [`drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop)，在这里 `String` 的作者可以放置释放内存的代码。Rust 在结尾的 `}` 处自动调用 `drop`。

这种模式在多个变量内存分配在 `Heap` 上时，其行为会变得不可预测。

#### 变量与数据交互的模式（一）：移动

在Rust中，多个变量可以采取不同的方式与同一个数据交互。



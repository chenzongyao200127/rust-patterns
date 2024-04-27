# 常见设计模式及其Rust实现

> 原文链接：邱奇琛 达坦科技DatenLord
> https://mp.weixin.qq.com/s/hUXewtjj_jK5_Ji7z1w-kg

本文提供了一个设计模式的综合概述，涵盖了设计模式的必要性，基本原则以及23种常见模式的概括性描述。结合Rust语言自身的特性，重点阐述了Rust中Builder,Combinator, RAII, Typestate(state machine), Command, Strategy和Visitor七种设计模式的应用。本文前半部分主要在阐述设计模式的思想和具体实例，如果读者已经很熟悉这一部分，可以自行跳转到第二小节。

## 01 速览设计模式
Q: 为什么需要设计模式
A: 减少耦合、利于复用、减少逻辑错误、快速开发、技术交流

Q：设计模式中的基本原则
A：SOLID
1. 单一职责 (Single Responsibility Principle, SRP)
   [每个类应只有一个引起变化的原因，即一个类应只负责一项任务或功能]
2. 开闭原则 (Open/Closed Principle, OCP)
   [软件实体（如类、模块、函数等）应对扩展开放，对修改关闭。这通常通过使用接口或抽象类实现，允许通过添加新的子类来扩展功能，而不需修改现有代码]
3. 里氏替换 (Liskov Substitution Principle, LSP)
   [子类对象应能够替换它们的父类对象而不破坏程序的正确性。设计子类时，应保证能够替代父类功能，不引入额外异常行为或削弱原有功能。]
4. 接口隔离 (Interface Segregation Principle, ISP)
   [应将大接口（提供多功能的接口）拆分为更小、更具体的接口，使实现接口的类只需关心它们真正需要的方法。]
5. 依赖倒置(Dependency Inversion Principle, DIP)
   [高层模块不应依赖于低层模块，两者都应依赖于抽象。抽象不应依赖于细节，细节应依赖于抽象。]
6. 迪米特法则（Law of Demeter）
   [如果两个实体不需要直接通信，就不应发生直接调用。例如，使用a.Method()而非a.b.Method()可以减少系统耦合，增强可修改性和扩展性。]

## 02 常见设计模式
下面简单介绍了各种模式的含义和常用场景。

### 建造型

- 工厂模式 Factory
定义一个创建对象的接口，让子类决定实例化哪一个类，使得类的实例化延迟到子类中。
当类的初始化需要依赖于具体情况和配置时，往往会用这种方式

- 抽象工厂Abstract Factory
定义一个接口来创建几种不同种类的类，其中每个种类可能有若干相互依赖的组件。用户无需指定需要用哪些特定的组件。
当需要创建一组相关或依赖的对象时，会使用这种方式。

- 建造者 Builder
定义一个接口，允许用户指定对象的步骤和过程；核心是分离的构造和表示两个步骤。
主要用于创建复杂的对象，或者希望用相同的创建过程可以创建不同的表示。

- 原型 Prototype
通过复制的方式创建对象。
使用于构造表示比较复杂的对象。

- 单例 Singleton
确保一个类只能有一个示例，并提供一个全局访问点来获取这个实例的设计模式
当全局只需要一个实例时常用这种模式（例如线程池）

## 结构型

- 适配器 Adapter
创建一个中间层，使得已有接口和新接口能交互。
一般已有接口是不可变或不方便修改的，例如glibc库等等

- 组合 Composite
将对象组合为树形结构，上层对象会对用下层方法完成某个操作。这使得对单个对象和组合对象的使用具有一致性。
一般在需要同时处理复杂对象和简单对象时，这种模式比较常用

- 桥接 Birdge
将抽象部分和实现部分通过组合的方式分离（组合替代继承）
很多时候都会用到这个模式，例如拆分庞大功能的类。

- 代理 Proxy
通过一个对象管理资源的访问
当访问资源开销较大，或需要权限检查等额外功能时，会使用这个模式

- 装饰器 Decorator
定义函数的接口与被覆盖函数的接口相同，用户可以无感调用拓展功能的接口
适合在原接口上无感拓展新的功能。

- 享元 Flywight
当有大量结构体共享一部分相同的数据时，将这部分数据独立出来，所有结构体共享这部分数据。
适用于不可变状态较多时节省内存。

- 外观 Facade
将许多抽象的小接口汇聚为一个对象，对象内部不对接口做任何处理
主要用于向外部提供统一接口

## 行为型

- 职责链 Chiain of responsibility
让请求以链式的形式逐一被处理。
解耦发送者和执行者时常用

- 命令 Command
将操作抽象为对象（通过函数指针等形式），从而允许使用不同的请求、队列或日志来参数化其他对象。
常用于参数化对象或者排队执行一些操作。

- 策略 Strategy
通过不同的上下文执行不同的算法变体，分离业务逻辑和具体的算法实现细节
通常用来抽象做一件事的多种方式。

- 迭代器 Iterator
隐藏迭代的细节，简化用户访问某个数据结构的代码
访问各种数据结构时都可用。

- 状态 State
将不同的状态设计为不同的类
当涉及到状态控制和转移的时候常用

- 备忘录 Memento
将数据结构的状态保存一份，用于后续恢复状态
常用于做snapshot等恢复状态的操作。

- 模板 Template
在父类定义一个算法框架，子类实现算法的特定部分。
常用于一些有固定执行逻辑的算法。

- 观察者 Observer
订阅监听模式，使用一个对象维护事件和对象间的一对多关系，当被监听事件产生时，会通知每个监听的对象
常用于需要处理事件，绑定回调函数的场景

- 访问者 Visitor
将算法与其作用的对象结构分离，通过在被访问的对象中添加一个接受方法来接受访问者，从而实现在不修改这些对象的类的情况下定义新的操作。
通常访问异构数据结构（例如树型）时常用

- 中介者 Mediator
引入一个中心对象来简化多个对象间的通信，从而减少这些对象之间的直接引用
通常用于解耦数据结构的的相互引用

- 解释器 interpret
定义了一种语法用于特定的问题，并通过一个解释器来解释语言中的句子，用于实现语言的语法或表达式的解析和执行。
当特定问题有特定的写法时，常用interpreter来解决。


# Rust中设计模式的应用

Rust设计之初就在语言嵌入了很多优雅的模式，例如模式匹配，所有权机制，零成本抽象等等。
下面，我们以Rust语言为例，介绍如何结合语言特性更好地利用设计模式。


# 01 Builder
Builder可能是Rust中应用最多的设计模式。
定义一个函数的接口不总是那么简单，尤其是当你遇到下面的情况时：
 - 有大量参数
 - 有多个可选参数
 - 只有某些参数的组合是有效的
加之Rust语言设计没有类似Python的关键字参数等特性，且不支持直接的函数重载，让我们处理复杂的接口定义更加棘手。

> Rust并不是没有重载，相反，trait中可以定义一个和类型实现中同名的方法，这就是一种重载。可以参考这篇文章：
> Abstraction without overhead: traits in Rust
> https://blog.rust-lang.org/2015/05/11/traits.html

例如，我们需要一个函数来处理打开文件的选项。
```rust
fn set_flag(
    is_read: bool,is_write: bool,is_append: bool,is_truncate: bool,is_create: bool,
) -> u32 {
    let mut flag = 0;
    if is_read {
        flag |= 0x0001;
    }
    // ....
    flag
}

fn main(){
    let flag = set_flag(true, true, false, false, false);
}
```

这种写法可读性很差，非常有可能导致你传错参数。

一种做法是将上面的参数组合为一个结构体，然后在结构体中实现约束。作为一个特殊的例子，我们可以直接用`bitflag`去抽象上面的参数。

~~~rust
extern crate bitflags;

bitflags! {
    struct Flags: u32 {
        const READ = 0b00001;
        const WRITE = 0b00010;
        const APPEND = 0b00100;
        const TRUNCATE = 0b01000;
        const CREATE = 0b10000;
    }
}

fn main() {
    let flag = Flags::READ | Flags::WRITE;
    println!("Flags: {:?}", flag);
}
~~~

另一种做法是使用builder。std中的`fs::OpenOptions`就使用了这种写法。

以用读/写权限打开foo.txt为例：

```rust
use std::fs::OpenOptions;

let file = OpenOptions::new()
    .read(true)
    .write(true)
    .open("foo.txt");
```

这种写法就十分的清晰。让我们来看看std中这个结构体的实现：

```rust
// unix OpenOptions: library/std/src/sys/pal/unix/fs.rs
// library/std/src/fs.rs

pub struct OpenOptions(OpenOptions);

impl OpenOptions {
    pub fn read(&mut self, read: bool) -> &mut Self {
        self.0.read(read);
        self
    }
    pub fn write(&mut self, write: bool) -> &mut Self {
        self.0.write(write);
        self
    }
    pub fn open<P: AsRef<Path>>(&self, path: P) -> io::Result<File> {
        // self._open调用了对应操作系统抽象层的open函数
        self._open(path.as_ref())
    }
}
```

可以看到，在设置完参数flag(read/write)后，会返回builder自己的可修改引用，这样就能使用链式调用了。

Builder这个设计模式的写法是相对比较固定的，因此有人开发了`derive_builder`这个crate，你可以通过添加attribute的形式快速为一个结构体添加它的builder。

```rust
#[derive(Default, Builder, Debug)]
#[builder(setter(into))]
struct Channel {
    token: i32,
    special_info: i32,
}

fn main() {
    let ch = ChannelBuilder::default()
        .special_info(42u8)
        .token(19124)
        .build()
        .unwrap();
    println!("{:?}", ch);
}
```

这样生成出来的Builder模式是比较固定的，读者自己参照这个模式去实现。
如果需要定制化地实现一个建造者，可以阅读Rust API指南中的几个tips后再进行实现。

使用 `derive_builder` crate 是一种非常高效的方法，可以快速为 Rust 结构体实现 Builder 设计模式。

这个 crate 通过简单的注解自动为你的结构体生成一个对应的 Builder 类型，从而减少了大量重复的代码编写工作。这种方法特别适用于需要构建复杂对象且对象的构建过程需要多个步骤或者配置选项的情况。

## 02 Combinator

Combinator是一个函数式编程的概念，但如今也在许多函数是一等公民的语言中。Combinator指的是一个函数仅通过其他函数的组合来完成工作。

在Rust中，最常见的应用是在异常处理中（Option and Result）。当然，上面的builder也是combinator一个特化形式。

Option和Result提供了大量combinator函数供用户转换和处理异常，同时提供了`?`语法糖简化常见的match语句。下面以option的API为例，简单介绍如何写出combinator风格的错误处理。

~~~rust
Option  -> Result :  Option::ok_or, Option::ok_or_else

Option<T>  ->  Option<T>  : Filter

Option<Option<T>>  -> Option<T> : flatten

Option<T> -> Option<U>  : map,map_or, map_or_else

Option<T>  -> Option<(T,U)> : zip, zip_with, unzip
~~~

Rust 的 `Option` 类型提供了一系列高阶函数，这些函数允许对可选值（`Option<T>`）进行强大且富有表现力的转换和操作。这些函数便于处理可选值，提供了一种机制来安全有效地转换、组合或甚至检索值。

下面将详细介绍你提到的每种转换，包括每种功能的简要说明和示例代码：

### 1. 将 `Option` 转换为 `Result`：`Option::ok_or` 和 `Option::ok_or_else`

- **`Option::ok_or`**：将 `Option<T>` 转换为 `Result<T, E>`，如果 `Option` 是 `None`，则使用提供的错误值。
- **`Option::ok_or_else`**：与 `ok_or` 类似，但如果 `Option` 是 `None`，使用一个函数来生成错误值。当错误值的构造成本较高时，这种方法非常有用。

```rust
let some_option: Option<i32> = Some(10);
let result: Result<i32, &str> = some_option.ok_or("没有值！");
println!("{:?}", result); // 输出：Ok(10)

let none_option: Option<i32> = None;
let result: Result<i32, &str> = none_option.ok_or_else(|| "没有值！");
println!("{:?}", result); // 输出：Err("没有值！")
```

### 2. 过滤 `Option<T>` -> `Option<T>`

- **`filter`**：对 `Option<T>` 应用一个断言（predicate）。如果值存在且断言返回 `true`，则返回未变的 `Option`。否则，返回 `None`。

```rust
let some_number: Option<i32> = Some(7);
let filtered_number = some_number.filter(|&x| x > 5);
println!("{:?}", filtered_number); // 输出：Some(7)

let filtered_number = some_number.filter(|&x| x > 10);
println!("{:?}", filtered_number); // 输出：None
```

### 3. 展平 `Option<Option<T>>` -> `Option<T>`

- **`flatten`**：将 `Option<Option<T>>` 转换为 `Option<T>`。如果外部 `Option` 是 `None`，或者它包含 `None`，结果是 `None`。

```rust
let option_of_option: Option<Option<i32>> = Some(Some(5));
let flattened: Option<i32> = option_of_option.flatten();
println!("{:?}", flattened); // 输出：Some(5)

let option_of_none: Option<Option<i32>> = Some(None);
println!("{:?}", option_of_none.flatten()); // 输出：None
```

### 4. 转换 `Option<T>` -> `Option<U>`：`map`, `map_or`, `map_or_else`

- **`map`**：通过对包含的值应用一个函数，将 `Option<T>` 转换为 `Option<U>`。
- **`map_or`**：对包含的值应用一个函数，或者在 `Option` 是 `None` 时返回一个默认值。
- **`map_or_else`**：类似于 `map_or`，但默认值由一个函数提供。

```rust
let some_string: Option<String> = Some("Hello".to_string());
let mapped_length = some_string.map(|s| s.len());
println!("{:?}", mapped_length); // 输出：Some(5)

let none_string: Option<String> = None;
let length_or_default = none_string.map_or(0, |s| s.len());
println!("{:?}", length_or_default); // 输出：0

let length_or_else = none_string.map_or_else(|| 0, |s| s.len());
println!("{:?}", length_or_else); // 输出：0
```

### 5. 组合 `Option<T>` -> `Option<(T,U)>`：`zip`, `zip_with`

- **`zip`**：将两个 `Option` 中的值组合成一个 `Option` 的元组。
- **`zip_with`**：类似于 `zip`，但使用一个函数来组合两个值。

```rust
let option_one: Option<i32> = Some(5);
let option_two: Option<&str> = Some("five");
let zipped = option_one.zip(option_two);
println!("{:?}", zipped); // 输出：Some((5,"five"))

let option_three: Option<i32> = None;
let zipped_with_none = option_one.zip(option_three);
println!("{:?}", zipped_with_none); // 输出：None
```

这些高阶函数极大地增强了在 Rust 中处理可选值的表达力和安全性，避免了常见的问题，如空指针错误和直接解包 `None` 值。通过这些功能，开发者可以更加便利和安全地操控可能不存在的值。

~~~rust
let maybe_some_string = Some(String::from("Hello, World!"));
// `Option::map` takes self *by value*, consuming `maybe_some_string`
let maybe_some_len = maybe_some_string.map(|s| s.len());
assert_eq!(maybe_some_len, Some(13));

let x: Option<&str> = None;
assert_eq!(x.map(|s| s.len()), None);
~~~

此外，我们可以对Option做bool操作。例如调用`Option::and`。如果调用的Option不为空，且传入的Option也不为空，则返回Option。

我们也可以传入一个函数。`Option::and_then()`传入一个函数，该函数接收一个Option内部的类型。如果Option非空，则执行这个函数。and_then能帮助你更好地处理异常。Rust example上有这样一个例子。例如，有下面两种结构体，Food和Day。

~~~rust
#[derive(Debug)] enum Food { CordonBleu, Steak, Sushi }
#[derive(Debug)] enum Day { Monday, Tuesday, Wednesday }
~~~

- 当Food为Sushi时，have_ingredients返回一个None，表示没有原料；
- 当Food为CordonBleu，have_recipe返回一个None，表示没有菜谱。

~~~rust
fn have_ingredients(food: Food) -> Option<Food> {
    match food {
        Food::Sushi => None,
        _           => Some(food),
    }
}

fn have_recipe(food: Food) -> Option<Food> {
    match food {
        Food::CordonBleu => None,
        _                => Some(food),
    }
}
~~~

接下来要实现一个cookable函数，表示一个食物既有菜谱，又有原料，如果使用match语句就会显得有点冗长。
我们可以使用option中的and_then将其转化为combinator。

~~~rust
fn cookable(food: Food) -> Option<Food> {
    have_recipe(food).and_then(have_ingredients)
}
~~~

如果have_recipe返回None，那么这个函数也会返回None。否则，它会执行have_ingredients再去检查。

事实上，and_then内部也是一个match语句。

虽然这样写编译的产物没有发生很大变化，但是使用combinator后整体的逻辑变得更加清晰，同时也有利于编译器做优化。

~~~rust
    pub fn and_then<U, F>(self, f: F) -> Option<U>
    where
        F: FnOnce(T) -> Option<U>,
    {
        match self {
            Some(x) => f(x),
            None => None,
        }
    }
~~~

**Summary**

Combinator（组合子）是函数式编程中的一个核心概念，它指的是一类通过组合其他函数来完成其功能的函数。

在函数式语言中，这种模式非常普遍，因为它们支持高阶函数（即函数可以接受其他函数作为参数或返回它们）。这种风格的编程允许开发者以声明性的方式表达复杂的逻辑，通常使代码更加简洁、更易于理解。

在支持一等公民函数的编程语言中，如 Rust、Haskell、JavaScript 等，组合子被广泛应用于各种场景，包括集合操作、异步编程、错误处理等。

Rust 是一种系统编程语言，它通过引入安全的内存管理和数据竞争保护机制，使得编写高效且安全的代码成为可能。Rust 通过 `Option` 和 `Result` 类型为错误处理和可选值处理提供了一套强大的工具，这些类型配备了多种组合子方法，允许开发者以函数式风格处理可能的错误或空值。

`Option` 类型拥有多个组合子方法，如 `map`, `and_then`, `filter`, `or_else` 等，这些方法使得在不解包 `Option` 的情况下操作和转换值变得简单安全。

与 `Option` 类似，`Result` 也提供了多种组合子方法，如 `map`, `map_err`, `and_then` 等，这些方法可以在不使用显式 `match` 语句的情况下转换和处理错误。

通过使用 `Option` 和 `Result` 的组合子，可以有效的简化错误处理代码。例如，使用 `?` 运算符可以快速从函数中返回错误，而避免显式的 `match` 语句，这使得代码更加简洁。

下面是一个简单的例子，展示如何使用 `Option` 和 `Result` 的组合子来处理可能的错误：

```rust
fn get_user_name(user_id: u32) -> Option<String> {
    Some("Alice".to_string())
}

fn process_user_name(user_id: u32) -> Result<(), String> {
    let user_name = get_user_name(user_id).ok_or("User not found")?;
    println!("Processing user: {}", user_name);
    Ok(())
}

fn main() {
    match process_user_name(123) {
        Ok(()) => println!("User processed successfully"),
        Err(e) => println!("Error: {}", e),
    }
}
```

在这个例子中，`get_user_name` 函数返回 `Option<String>`，使用 `ok_or` 组合子将其转换为 `Result<String, &str>`，然后 `?` 运算符用于在 `None` 的情况下提前返回错误。这样的组合子风格的错误处理使得代码逻辑更加连贯和易于维护。

总体而言，Rust 通过组合子提供的丰富的 API 使得函数式风格的错误处理成为可能，从而增强了代码的表达力和安全性。

# 03 RAII

RAII指的是 `Resource Acquisition Is Initialization`，意思是资源初始化在对象的构造器中完成，最终化（资源释放）在析构器中完成。

RAII可以简化类似C中复杂的异常处理，同时又可以避免程序员一些逻辑错误导致资源没有释放。

利用RAII的思想，可以设计一类`Guard`结构体，它们提供对资源访问的唯一安全接口。Rust中Mutex等锁就利用了这一思想。

一个C风格的锁如下面的代码所示：

```rust
struct Mutex<T> {
   data: UnsafeCell<T>,
   system_mutex: SystemMutex,
}

impl<T> Mutex<T> {
   pub fn new(t: T) -> Self { /* .. */}

   pub fn lock(&self) -> &mut T {
      self.system_mutex.lock();
      unsafe { &mut *self.data.get() }
   }

   pub fn unlock(&self) {
      self.system_mutex.unlock();
   }
}
```

用户需要调用unlock来解锁。这么写可能有个问题：

调用者调用lock后，忘记unlcok了，导致死锁，这种情形在多出口的while循环中很常见。

在标准库中，调用Mutex::lock后，不会返回对资源的直接引用，而是一个被MutexGuard保护的资源的引用：

~~~rust
pub struct MutexGuard<'a, T: ?Sized + 'a> {
    lock: &'a Mutex<T>,
}

impl<'a, T> MutexGuard<'a, T> {
    fn new(lock: &'a Mutex<T>) -> Self {
        lock.system_mutex.lock();
        MutexGuard { lock }
    }

     fn get(&mut self) -> &mut T {
        &mut *self.lock.get_mut()
    }
}
~~~

调用者可以调用guard的get方法来获取内部引用。

同时，Guard还实现了drop特性：

~~~rust
impl<'a, T:?Sized> Drop for MutexGuard<'a, T> {
    fn drop(&mut self) {
        self.lock.system_mutex.unlock();
    }
}
~~~

当Guard离开作用域时，就会自动调用unlock方法解锁。
除了使用Guard的方式保护内部变量，还可以提供回调接口的方式进行保护。
比如：

~~~rust
impl<T> Mutex<T> {
    pub fn lock_closure(&self, mut f: impl FnMut(&mut T)) {
        self.system_mutex.lock();
        f(self.get_mut());
        self.system_mutex.unlock();
    }
}
~~~

Guard保护的一般是完整的结构体，如果想要用户只能访问结构体内部的某些字段，除了可以用多个锁的方案，可以尝试`parking_lot`中的`try_map`方法。

~~~rust
use parking_lot::Mutex;

struct MyStruct {
   important_data: i32,
   less_important_data: String,
}

let mutex = Mutex::new(MyStruct {
   important_data: 42,
   less_important_data: "Hello".to_string(),
});

// 只锁定并访问 important_data 字段
let important_data_guard = mutex.lock().try_map(|data| {
   Some(&mut data.important_data)
}).unwrap();

// 在这里对 important_data 进行操作
*important_data_guard += 1;

// important_data_guard 超出作用域，自动释放锁
~~~

Guard最常见的用途是在锁资源的保护中，但并不意味着这种模式只能用于锁，当我们需要实现一些延迟访问或者批量处理的操作也可以用这个模式。

~~~rust
struct BatchGuard<'a, T> {
   data: &'a mut Vec<T>,
   batch_operations: Vec<T>,
}

impl<'a, T> BatchGuard<'a, T> {
   pub fn new(data: &'a mut Vec<T>) -> Self {
      BatchGuard {
            data,
            batch_operations: Vec::new(),
      }
   }

   pub fn add(&mut self, item: T) {
      self.batch_operations.push(item);
   }
}

impl<'a, T> Drop for BatchGuard<'a, T> {
   fn drop(&mut self) {
      self.data.extend(self.batch_operations.drain(..));
   }
}
~~~

# Typestate

typestate是Rust中另一个经典的模式。传统的状态模式用一个枚举值来表示状态，而Rust中你可以利用`enum`的特性来实现使用基于类型的状态管理。
这个模式被称为Typestate，也有称为session type。
Typestate相较于传统的枚举值有很多好处：
 - 防止用户从非开始状态进入
 - 防止发生错误的转换
 - 特别地,Rust的所有权机制可以消耗掉状态，使得用户手上的状态发生转换后就不会存在。

下面以一个文件打开、读取操作为例，简单介绍typestate模式。

open -> reading -> eof -> closed

一个文件的读写操作可能涉及到上面的状态转移图。我们可以用Rust抽象这一过程。完整的代码见此：

~~~rust
use std::ffi::CString;

pub struct File {
    fd: libc::c_int,
    buf: Vec<u8>,
}

impl File {
    pub fn open(path: String) -> Result<File, String> {
        let fd = unsafe { libc::open(CString::new(path).unwrap().as_ptr(), 0) };
        if fd == -1 {
            /* assume errno_string() reads errno and converts it
             * into a Rust string through strerror() */
            Err(std::io::Error::last_os_error().to_string())
        } else {
            Ok(File {
                fd,
                buf: Vec::with_capacity(1024),
            })
        }
    }

    pub fn read(mut self, bytes: usize) -> Result<usize, String> {
        // Ensure buf has enough space
        self.buf.reserve(bytes);
        // Read the file
        let bytes_read =
            unsafe { libc::read(self.fd, self.buf.as_mut_ptr() as *mut libc::c_void, bytes) };

        // If the read failed, then immediately return an error
        if bytes_read == -1 {
            return Err(std::io::Error::last_os_error().to_string());
        }

        // Increase length of the vector for the leements copied
        unsafe { self.buf.set_len(self.buf.len() + bytes_read as usize) };
        Ok(bytes_read as usize)
    }
}
~~~

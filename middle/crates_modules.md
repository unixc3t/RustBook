#Crates 和 Modules
- - -
当一个项目变得越来越大，推荐的软件工程的最佳实践方式就是将它分割成更小的部分，把他们组合在一起。
良好接口的设计也是很重要的,所以一些功能是私有的，一些事公有的。Rust有一个模块系统来处理这些事情。


#基础概念: Crates 和 Modules

- - -

Rust有两个与模块系统相关的独特术语:crate和module,crate的意思和其他语言中的library和package
一样。Rust的包管理系统的名字是“Cargo”:你使用Cargo分享你的crates给其他人。基于项目，Crates
可以生成可执行程序或者library.

每个crate有一个隐藏的根模块包含了crate的代码。在根模块下你可以定义一个子模块树，你可以使用modules
来区分你的代码和crate本身的代码。

例如，我们创建一个叫phrase的crate，将提供我们不同语言的各种短语。为了使例子简单。我们只提供
"greeting"和"farewells"两种短语,使用英语和日语作为短语的语言。下面是我们的module布局:


                                      +-----------+
                                  +---| greetings |
                                  |   +-----------+
                    +---------+   |
                    | english |---+
                    +---------+   |   +-----------+
                    |             +---| farewells |
      +---------+   |                 +-----------+
      | phrases |---+
      +---------+   |                  +-----------+
                    |              +---| greetings |
                    +----------+   |   +-----------+
                    | japanese |---+
                    +----------+   |
                                   |   +-----------+
                                   +---| farewells |
                                       +-----------+

在上面的例子中，我们crate的名字是phrases,其余的都是module,他们的组织结构是一个树，
以根create创建分支，这个树的根就是phrases自己。

现在我们有一个计划,以代码的形式定义这些模块。我们使用Cargo生成一个crate：

    $ cargo new phrases
    $ cd phrases


如果你还记得,以上代码将生成一个简单的项目

    $ tree .
    .
    ├── Cargo.toml
    └── src
    └── lib.rs

    1 目录, 2 文件



**src/lib.rs是我们的根crate，相当于上图中的phrases



#定义modules

使用**mod**关键字定义我们的模块，在src/lib.rs中像下面这样编写:

    // in src/lib.rs

    mod english {
        mod greetings {

        }

        mod farewells {

        }
    }

    mod japanese {
        mod greetings {

        }

        mod farewells {

        }
    }

mod关键字后面是模块的名字。模块的命名采用Rust其它标识符的命名惯例：lower_snake_case。
在大括号中（{}）是模块的内容。

在一个mod内部，可以声明子模块，我们使用两个冒号::引用子模块，我们嵌套的模块是**english::greetings, english::farewells, japanese::greetings**,和**japanese::farewells**，因为子模块位于父模块的命名空间下,
命名没有冲突，english::greetings and japanese::greetings 是不同的，即使他们的名字都包含
greetings

因为这个包装箱的根文件叫做lib.rs，且没有一个main()函数。Cargo会把这个包装箱构建为一个库：

    $ cargo build
    Compiling phrases v0.0.1 (file:///home/you/projects/phrases)
    $ ls target
    deps  libphrases-a7448e02a0468eaa.rlib  native

libphrase-hash.rlib就是编译后的crate，在我们了解如何使用这个crate之前，
先让我们把它拆分为多个文件。


#多文件crates

- - -

如果每个crate仅仅是一个文件,这些文件将会变得很大，分割crate到多个文件很简单。Rust有两中方法支持这么做


除了这样定义一个模块外:

    mod english {
        // contents of our module go here
    }

还可以使用下面方式定义:

    mod english;

如我们这么做了,Rust希望找到一个english.rs文件或者english/mod.rs这种结构的文件，里面包含了
我们的模块内容.

    // contents of our module go here

注意这些文件，你不需要重新声明这些模块。他们已经在开始的mod处理好了。


使用这两种技术，我们可以将我们的crate拆分为两个目录和七个文件：


    $ tree .
    .
    ├── Cargo.lock
    ├── Cargo.toml
    ├── src
    │   ├── english
    │   │   ├── farewells.rs
    │   │   ├── greetings.rs
    │   │   └── mod.rs
    │   ├── japanese
    │   │   ├── farewells.rs
    │   │   ├── greetings.rs
    │   │   └── mod.rs
    │   └── lib.rs
    └── target
    ├── deps
    ├── libphrases-a7448e02a0468eaa.rlib
    └── native


src/lib.rs是我们的根crate，里面内容

    // in src/lib.rs

    mod english;

    mod japanese;

这两句声明告诉Rust查看src/english.rs 和 src/japanese.rs或者
src/english/mod.rs 和 src/japanese/mod.rs.在这个例子中，因为我们的模块有子模块，所以我们选择第二种.src/english/mod.rs 和 src/japanese/mod.rs ,看起来像这样

    // both src/english/mod.rs and src/japanese/mod.rs

    mod greetings;

    mod farewells;
    
这两句声明语句，会告诉Rust查看 src/english/greetings.rs 和 src/japanese/greetings.rs 或者 src/english/farewells/mod.rs 和 src/japanese/farewells/mod.rs.因为这两个子模块没有他们的子模块，
所以我们选择这种 src/english/greetings.rs 和 src/japanese/farewells.rs. 

src/english/greetings.rs 和 src/japanese/farewells.rs的内容都是空的，让我们加入一些函数

src/english/greetings.rs:

    // in src/english/greetings.rs

    fn hello() -> String {
        "Hello!".to_string()
    } 
    
    
src/english/farewells.rs:

    // in src/english/farewells.rs
    
    fn goodbye() -> String {
        "Goodbye.".to_string()
    }
 

src/japanese/greetings.rs:

    // in src/japanese/greetings.rs

    fn hello() -> String {
        "こんにちは".to_string()
    }
    

你可以拷贝粘贴页面上的代码,或者自己写一些类似的代码.知道"konnichiwa"是什麽意思,对于学习module来说不重要


src/japanese/farewells.rs:

    // in src/japanese/farewells.rs

    fn goodbye() -> String {
        "さようなら".to_string()
    }
    
在我们的crate里已经有了函数了,让我们在其他的crate里使用这个.

#导入外部crates
- - -
我们有一个库crate。让我们创建一个可执行crate并且导入使用我们的库。

创建src/main.rs文件并且里面写入，如下代码(不能完全编译)

    // in src/main.rs

    extern crate phrases;

    fn main() {
        println!("Hello in English: {}", phrases::english::greetings::hello());
        println!("Goodbye in English: {}", phrases::english::farewells::goodbye());
    
        println!("Hello in Japanese: {}", phrases::japanese::greetings::hello());
        println!("Goodbye in Japanese: {}", phrases::japanese::farewells::goodbye());
    }
 
 
**extern crate**声明语句告诉Rust 我们需要编译链接**phrase** crate。然后我们就可以使用phrase的模块了,像上面我们讲过的，你可以使用两个冒号来引用子模块和里面的函数。

同时,Cargo认为src/main.rs是一个二进制crate的根，而不是一个库crate，我们的包里现在有两个crate:src/lib.rs和src/main.rs。
对于一个可执行的crate这种形式很平常:多数功能函数在一个库crate里，可执行crate使用这个库。顺便说，其他程序也可以使用，这是一种使关注点分离的好方式

还是不能工作，我们得到四个错误。像下面这样:
    
     $ cargo build
       Compiling phrases v0.0.1 (file:///home/you/projects/phrases)
    /home/you/projects/phrases/src/main.rs:4:38: 4:72 error: function `hello` is private
    /home/you/projects/phrases/src/main.rs:4     println!("Hello in English: {}", phrases::english::greetings::hello());
                                                                               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    note: in expansion of format_args!
    <std macros>:2:23: 2:77 note: expansion site
    <std macros>:1:1: 3:2 note: in expansion of println!
    /home/you/projects/phrases/src/main.rs:4:5: 4:76 note: expansion site
    
默认情况下 Rust中所有都是私有的，让我们再谈论一些更深入的东西

#导出公共接口
 - - -
Rust允许你精确控制你的接口哪部分是公有的。默认是私有的，为了使其公有你需要**pub**关键字。让我们首先看**english模块**，减少src/main.rs的代码。像这样:

    // in src/main.rs

    extern crate phrases;

    fn main() {
        println!("Hello in English: {}", phrases::english::greetings::hello());
        println!("Goodbye in English: {}", phrases::english::farewells::goodbye());
    }
    
在src/lib.rs文件中。我们给english模块声明加上 pub关键字

    // in src/lib.rs

    pub mod english;

    mod japanese;

在src/english/mod.rs中，两个都加上pub

     // in src/english/mod.rs

    pub mod greetings;

    pub mod farewells;
    
在src/english/greetings.rs 中，给fn声明语句加上pub

    // in src/english/greetings.rs

    pub fn hello() -> String {
        "Hello!".to_string()
    }
    
同样src/english/farewells.rs:
    
    // in src/english/farewells.rs

    pub fn goodbye() -> String {
        "Goodbye.".to_string()
    }
    
现在我们编译，有一些没有使用japanese发出的警告:
    
    $ cargo run
       Compiling phrases v0.0.1 (file:///home/you/projects/phrases)
    /home/you/projects/phrases/src/japanese/greetings.rs:1:1: 3:2 warning: code is never used: `hello`, #[warn(dead_code)] on by default
    /home/you/projects/phrases/src/japanese/greetings.rs:1 fn hello() -> String {
    /home/you/projects/phrases/src/japanese/greetings.rs:2     "こんにちは".to_string()
    /home/you/projects/phrases/src/japanese/greetings.rs:3 }
    /home/you/projects/phrases/src/japanese/farewells.rs:1:1: 3:2 warning: code is never used: `goodbye`, #[warn(dead_code)] on by default
    /home/you/projects/phrases/src/japanese/farewells.rs:1 fn goodbye() -> String {
    /home/you/projects/phrases/src/japanese/farewells.rs:2     "さようなら".to_string()
    /home/you/projects/phrases/src/japanese/farewells.rs:3 }
         Running `target/phrases`
    Hello in English: Hello!
    Goodbye in English: Goodbye.
    
    
现在我们的函数是公有的，我们可以使用他们，棒极了。然而，使用**phrases::english::greetings::hello()**非常长而且冗余，Rust有另一个关键字导入名称到当前在作用域，所以你可以是用短名字来引用它。让我们学习使用use

#使用use导入模块
- - -
Rust有一个use关键字,允许我们导入名字到我们的作用域内。让我们修改src/main.rs像下面这样

    // in src/main.rs

    extern crate phrases;
    
    use phrases::english::greetings;
    use phrases::english::farewells;
    
    fn main() {
        println!("Hello in English: {}", greetings::hello());
        println!("Goodbye in English: {}", farewells::goodbye());
    }
    
两条use语句导入每个模块到本地作用域，所以我们可以使用更短的名字来引用函数,通常来说，当导入功能函数时候，我们推荐只导入模块，不要直接导入函数。换句话说，像下面这样:

    extern crate phrases;

    use phrases::english::greetings::hello;
    use phrases::english::farewells::goodbye;
    
    fn main() {
        println!("Hello in English: {}", hello());
        println!("Goodbye in English: {}", goodbye());
    }
    
这么做不推荐,这么做更容易导致名称空间冲突。我们的程序很短，影响不大。但是当程序变大，这么做将会产生问题，如果名称发生冲突.Rust将会给出
一个编译错误,例如，我们使用japanese的公有函数，试着这么做:

    extern crate phrases;

    use phrases::english::greetings::hello;
    use phrases::japanese::greetings::hello;
    
    fn main() {
        println!("Hello in English: {}", hello());
        println!("Hello in Japanese: {}", hello());
    }
    
Rust将会给我们一个编译时错误：

       Compiling phrases v0.0.1 (file:///home/you/projects/phrases)
    /home/you/projects/phrases/src/main.rs:4:5: 4:40 error: a value named `hello` has already been imported in this module
    /home/you/projects/phrases/src/main.rs:4 use phrases::japanese::greetings::hello;
                                              ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    error: aborting due to previous error
    Could not compile `phrases`.
        
 如果我们导入来自同一个模块的多个名字,我们不需要输入两次，Rust有一个段语法可以这么写:
 
     use phrases::english::greetings;
     use phrases::english::farewells;
    
你可以使用大括号:
    
    use phrases::english::{greetings, farewells};
    
这两种声明是等价的，但是第二种更简洁    

#使用pub use重导入

你不仅可以使用use缩短标识符，你还可以将crate里面的函数导入到另一个模块内，这样允许你提供一个扩展接口，并不会映射你的代码层级结构

让我们看一个例子，修改src/main.rs文件像这样:

    // in src/main.rs

    extern crate phrases;
    
    use phrases::english::{greetings,farewells};
    use phrases::japanese;
    
    fn main() {
        println!("Hello in English: {}", greetings::hello());
        println!("Goodbye in English: {}", farewells::goodbye());
    
        println!("Hello in Japanese: {}", japanese::hello());
        println!("Goodbye in Japanese: {}", japanese::goodbye());
    }
    
修改src/lib.rs文件。将japanese公开
    // in src/lib.rs
    
    pub mod english;
    
    pub mod japanese;
    
将src/japanese/greetings.rs中的函数公开:
    // in src/japanese/greetings.rs
    
    pub fn hello() -> String {
            "こんにちは".to_string()
    }
然后是src/japanese/farewells.rs:
    // in src/japanese/farewells.rs
    
    pub fn goodbye() -> String {
        "さようなら".to_string()
    }
        
最后修改 src/japanese/mod.

    // in src/japanese/mod.rs

    pub use self::greetings::hello;
    pub use self::farewells::goodbye;
    
    mod greetings;
    
    mod farewells;

**pub use**声明将函数带入了我们的模块空间中，因为pub use 在我们的japanese模块中，我们现在有一个phrases::japanese::hello() 函数 和 a phrases::japanese::goodbye() 函数，即使他们的代码位于phrases::japanese::greetings::hello() 和 phrases::japanese::farewells::goodbye()中，内部组织结构不能反映我们的扩展接口

对于每个我们想放入javapanese模块中的函数我们都有一个pub use。我们可以使用通配符语法来包含所有来自greetings的函数到当前作用域:
**pub use self::greetings::*. **

self是什么，默认情况下，use声明表示绝对路径，从你的根crate开始， self表示相对于你当前所在的位置，有一个更特殊的use形式:你可以使用
**use super:: ** 引用你当前位置的上一级，一些人喜欢把self当作**.**，super 当作**..**,像许多shell显示当前目录和父目录那样


没有使用use，表示路径是相对的，foo::bar() 引用了foo内部函数，foo的位置相对于我们所在位置，如果前缀加上了::,::foo::bar(),它引用了一个不同的foo,一个相对于你的根crate的路径。

我们编译运行:
       $ cargo run
       Compiling phrases v0.0.1 (file:///home/you/projects/phrases)
         Running `target/phrases`
    Hello in English: Hello!
    Goodbye in English: Goodbye.
    Hello in Japanese: こんにちは
    Goodbye in Japanese: さようなら 
    
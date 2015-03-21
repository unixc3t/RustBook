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
    
在我们的crate里已经有了函数了,让我们在其他的crate里使用这个











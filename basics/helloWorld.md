[安装Rust][ins]

[ins]: install_rust.md

#Hello,World!

---

现在你已经安装了Rust，让我们来写第一个程序，就是在屏幕上打印“Hello，world”，这也是你学其他语言时都要写的一个程序，写一个简单的程序来验证你的编译器不仅是安装好了，而且也正常工作，打印信息到屏幕上也是常规手段

首先我们需要新建一个文件然后写代码，我通常在我的家目录里新建一个项目的根目录。把我的所有项目都放里面，对于rust来说你的代码在哪都可以。

这引出了另一个我们关心的事情，这本指南假设你会基本的命令行操作，rust不需要你掌握大量的命令行知识，在这个语言更加完善之前，ide支持比较差，rust对写代码的工具和文件位置没有特殊要求

让我们在项目根目录建立好文件夹

    $ mkdir ~/projects
    $ cd ~/projects
    $ mkdir hello_world
    $ cd hello_world
如果你用的是window系统并且没使用PowerShell，这个～不会起作用，请查询你自己shell文档来解决这个问题

让我们创建一个源码文件,在后面的例子中我使用**editor 文件名**代表编写一个文件，你可以使用你喜欢的任何方式，我给文件起名 main.rs

    $editor main.rs
 Rust文件总是以.rs作为扩展名，如果你使用多个单词给文件命名，请使用下划线，hello_World.rs比helloworld.rs好一些

 现在你已经打开你的文件了写入一下代码

    fn main() {
        println!("Hello,world!");
    }

保存文件，然后在你的终端输入

    $ rustc main.rs
    $ ./main # or main.exe on Windows
    Hello, world!

你也可以在[play.rust-lang.org][r]来运行这些例子，当你鼠标在你想运行的代码上时点击左上角的**evauluate**

[r]:https://play.rust-lang.org/

成功了，让我们详细的看一下刚才发生的

    fn main() {

    }

上面的几行代码表示定义了一个Rust函数，main是一个特殊的函数，是每个Rust写的程序的入口，第一行代码是说“我声名了一个叫main的函数没有参数，没有返回值”，
如果有参数的话我们就在圆括号 ( 和 ) 中声明，因为这个函数没有返回任何值，所以我们可以省略返回类型，我们在后面说它

你或许注意到了函数被包裹在花括号中，Rust的函数体用花括号包裹，在函数后面键入一个空格 然后写一个左花括号的方式也是比较好的风格，

下面这句

    println!("Hello, world!");

这行就是我们程序运行的所有代码，这行有许多重要的细节，代码开头是四个空格不是tab键，请配置你的编辑器用四个空格代替tab插入，我的一些配置[建议][f]


第二点就是println!()部分，这个叫做Rust宏，在Rust中是元编程处理机制，如果用一个函数代替，就像这样 println().现在来说。我们不需要关注他们有什么不同
当你看到一个！知道这个就行，表示你调用了一个宏来代替一个函数，Rust实现了println!作为宏而不是函数是有原因的，那是高级话题，我们稍后谈论宏的时候你可以了解更多
注意一点，Rust的宏和C语言的不一样，如果你用过C语言的宏，不要担心宏，我们后面会详细了解它，现在只要相信我们就行了

接下来，"hello,world"是一个字符串，在系统级编程语言中字符串异常复杂，这是一个**静态分配**的字符串，稍后我们谈论不同的分配方式，我们传递这个字符串
作为参数给println!打印到屏幕上，很简单

最后，最后这行结尾有一个分号，Rust是一个面向表达式的语言，意味着大多数东西都是表达式，这个**;**作为表达式的结束，下一个表达式将开始，大多数rust代码用一个**;**
结尾，指南后面章节我们深入了解，

最后，编译运行我们的程序，我们使用rustc编译器，将文件名传递给编译器

    $ rustc main.rs

这个方式类似gcc或者clang ,如果你有c/c++经验，Rust生成一个二进制可执行文件，使用ls命令查看

    $ ls
    main  main.rs

window下

    $ dir
    main.exe  main.rs

这有两个文件：我么的原文件以.rs扩展名结尾，一个可执行文件(window平台下就是main.exe,其他平台是main)

    $ ./main  # or main.exe on Windows

在我们的终端屏幕上打印 Hello,World!

如多你用过动态编程语言Ruby，python，或者javascript，你或许不会使用这种2步方式运行程序，Rust是一个预编译语言，意味着你可以编译一个程序。
给其他人，他们不需要安装Rust，如果你给他一个.rb or .py or .js文件，他们还需要安装ruby/python/javascript运行环境,你仅仅是需要一个命令编译
运行你的程序，在语言设计时候要平衡个方面，rust做了取舍。

恭喜你，你已经正式的写了一个Rust程序，意味着你已经是一个rust程序眼了，欢迎加入

下一步，我很乐意想妳介绍另一个工具**Cargo** ,使用它编写现实中的Rust程序，对于简单的事情使用rustc是很高效的，但是你的项目不断扩大，你想要某些东西
来管理所有的选项，并且容易分享给其他人和项目

[Hello,Cargo!][ca]

[ca]: hello_Cargo.md




[f]: https://github.com/rust-lang/rust/tree/master/src/etc/CONFIGS.html



[你好，世界][hw]

[hw]: helloWorld.md

#你好,Cargo

- - -

Cargo是一个Rustaceans用来帮助他们管理项目的工具，Cargo目前是alpha阶段，和rust一样，仍然在开发中，
但是它已经足够可以管理多数Rust项目了，我们假设Cargo从头管理这些项目

cargo负责三件事，编译代码，下载代码需要的依赖，编译这些依赖，起初，你的程序不需要依赖包，
所以我们只需要
使用它的功能的第一部分，最后。我们将加入更多，所以一开始就用cargo，将会变得很容易

如果你安装Rust通过离线安装。那么你已经安装了Cargo，如果你通过其它方式，你或许想看一下[Cargo文档][c1]
来安装他

[c1]: https://github.com/rust-lang/cargo#installing-cargo-from-nightlies

让我们把Hello world 变成Cargo管理的项目

cargo化我们的项目，需要做2件事，新建一个**Cargo.toml**配置文件，把我们的源文件放到指定的地方，
我们首先做第一步

	$ mkdir src
	$ mv main.rs src/main.rs

cargo认为你的代码在一个叫src目录的文件夹里，顶级目录放一些其他的文件，例如Readme，许可证文件，
任何和你代码不相关的文件，cargo是我们的项目整洁，即可以放很多文件，又方便管理，

下一步我们编辑配置文件：
	$ editor Cargo.toml
 
注意要文件名字正确，需要使用大写的C

写入如下代码
	[package]

	name = "hello_world"
	version = "0.0.1"
	authors = [ "Your name <you@example.com>" ]

	[[bin]]

	name = "hello_world"
	
	
文件需要遵循[TOML][tmol]格式，看下面的说明：

[tmol]: https://github.com/toml-lang/toml
	
TOML目标是成为一个最小巧的配置文件格式以达到容易阅读理解。toml被设计成一个容易映射成一个哈希表，TOML
应该很容易被解析数据结构，在多种语言中。

TOML类似于ini文件，但是有一些好的扩展。
	
不管怎么说，文件中有两个表格**package**和bin ，第一个是告诉Cargo你的包的元数据，
第二个告诉Cargo我们想构建一个二进制程序不是一个函数库(或者两个都要)，并且叫什么名字
你已经有了这个文件了，让我们准能被编译，：

	$ cargo build
	Compiling hello_world v0.0.1 (file:///home/yourname/projects/hello_world)
	$ ./target/hello_world
	Hello, world!
   
我们用cargo编译好了我们的项目，然后运行它./target/hello_world.
相比较我们简单的使用**rustc** ,cargo看起来没有方便太多,想一下以后，
当我们项目有更多文件时候，我们将多次使用rustc，并且传递一堆参数给它，但是使用cargo，
当我们的项目变大后，我们只需要用cargo构建，它会处理好

你或许会注意到cargo创建一个叫做Cargo.lock的文件。
    [root]
    name = "hello_world"
    version="0.0.1"
这个文件被cargo用来跟踪记录你应用程序的依赖关系，目前为止，我们没有很多。所以他只一点
你不需要关心这个文件的创建，cargo会负责

就这些了。我们成功使用cargo构建了hello_world，虽然我们的程序很简单，他已经使用了在你余下的rust
生涯里将会用到的大部分工具

现在你已经了这个工具，让我们来实实切切学习更多关于rust语言的东西，这些基础将会在你学习rust
的时间里更好的帮助你

[变量绑定][vb]

[vb]: http://doc.rust-lang.org/book/variable-bindings.html
    
    
    
    


	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	

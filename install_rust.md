[2.I基础部分][basics]
[basics]: basics.md "基础部分"

###安装Rust

- - -

第一步使用Rust安装它！有许多方式安装Rust，但是最简单的事使用**rustup**脚本安装，如果你使用linux系统或者一台Mac，你需要这么做(注意你不需要输入$，这个只是表示每个命令开头)

       
    $ curl -L https://static.rust-lang.org/rustup.sh | sudo sh

如果你担心使用**curl | sudo sh**不安全,请阅读我们下面的免责声明，使用两步的安装方法检查我们的安装脚本

    $ curl -L https://static.rust-lang.org/rustup.sh -O
    $ sudo sh rustup.sh
    
如果你使用Windows，请下载[32-bit installer][32]或者[64-bit installer][64]安装
[32]: https://static.rust-lang.org/dist/rust-nightly-i686-pc-windows-gnu.exe
[64]: https://static.rust-lang.org/dist/rust-nightly-x86_64-pc-windows-gnu.exe

如果你决定不想要Rust了,我们很难过不过没关系，不是每个编程语言都适合每个人，只需要运行下面脚本

	$ sudo /usr/local/lib/rustlib/uninstall.sh
	
如果你使用的是windows installer安装的，只需要再运行它一次，他会给你卸载选项

你可以运重复运行这个脚本在你想升级Rust的任何时间，Rust仍然是1.0测试版，如果人们觉得你使用的是一个最近的版本的话

这使我注意另一点：一些人，有些事里多当然的，当我们告诉他使用**curl | sudo sh**时会很困惑不安，也是应该的，当你运行这个命令，你相信维护Rust的人员不会去黑你的电脑做坏事，直觉敏锐，如果你是这样的人，请查看[构建Rust][build]文档或[官方二进制下载版本][offcial]，我们保证这个方法不会永远是Rust的安装方法，它仅仅是当Rust在alpha阶段时，让人们升级Rust的最简单方法
[build]:https://github.com/rust-lang/rust#building-from-source
[offcial]:http://www.rust-lang.org/install.html
我们也提到了官方支持的平台

* Windows (7, 8, Server 2008 R2)
* Linux (2.6.18 or later, various distributions), x86 and x86-64
* OSX 10.7 (Lion) or greater, x86 and x86-64

我们在这些平台上进行了大规模测试，还有另一些平台，例如安卓，这些是最有可能工作稳定的，因为有最多的测试

最后有关wndows说明，rust曾经考虑windows作为第一平台在release发布时，说实话，Windows上的体验并不像Linux/OS X上的体验一样，我们仍继续努力，如果有些东西不能用，它是一个bug，如果有的话请告诉我们，对于windows的每一个提交都会像其他平台一样被测试

如果你已经安装了rust，你可以打开一个控制台，输入

	$ rustc --version

你会看到一些类似这样的输出信息

	rustc 1.0.0-nightly (f11f3e7ba 2015-01-04 20:02:14 +0000)
	
	
如果你做的结果和这个一样，祝贺你，rust安装成功

如果不是这样的话，有很多地方你可以得到帮助，最简单的是[**rust IRC**][irc]频道,通过[Mibbit][m]
访问，点击链接与其他Rustaceans(我们给自己取的傻名字)我们能把你解决，其他好的资源有[the /r/rust subreddit][other],和[Stack Overflow][sf]

[2.2.Hello，World][hw]
[hw]:hello_world.md
[sf]:http://stackoverflow.com/questions/tagged/rust
[other]:http://www.reddit.com/r/rust
[m]:http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust
[irc]: irc://irc.mozilla.org/#rust


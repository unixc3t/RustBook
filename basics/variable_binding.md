[你好,Cargo!][hc]

[hc]: hello_Cargo.md

#变量绑定
- - -

首先我们先学习**变量绑定**，他们看起来像这样:

    fn main() {
        let x = 5;
    }

每个例子都写*fn main() {*有点冗余,后面例子中我们将忽略它，如果你继续学习下去，务必自己补充上*main()*函数，而不是丢掉它。
否则你会得到错误提示

在许多语言中，这个叫做*变量*，但是Rust的变量绑定有自己的特点和优势，Rust有一个很强大的特性叫做*模式匹配*，稍后我们会详细了解。
let表达式的左手边的部分是一个完全匹配，不仅仅是一个变量名，这意味着我们能这样做，像这样：

    let (x,y) = (1,2);

这个表达式执行后，x将是1，y将是2，模式真的很强大，但是目前我们只能说到这，让我们先暂时记住这个，然后继续学习。

Rust是一个静态类型的语言，意味着我们提前指定类型，但是为什么第一个例子能编译呢？Rust有一种叫做*类型推断*的能力，可以
计算出数据的类型，Rust不需要你指出他们的类型。

不过既然我们想，也可以加入类型标识，后面紧挨着敲一个冒号:
    let x:i32 =5;
如果让你大声的向其他人读出，你应该说"x 绑定了i32类型，值是5".

在这个例子中，我们选择x作为一个32位的有符号整型，Rust有很多不同的基本整型类型，对于有符号的整型用i开头，对于无符号的整型用u开头，
整型的大小可能是8位，16位，32位和64位。

后面的例子，我们可能会给类型做个注释。像这样

    fn main() {
        let x = 5; // x: i32
    }


注意这个注解和你使用的let语法相似，这种注释在rust中是不常用的，但是我们偶会会这样写帮助你理解rust的类型推断

默认情况，绑定的变量是不可变得，下面代码不能通过编译
    
    let x=5;
    x=10;
    
会提示你这样的错误

    error: re-assignment of immutable variable `x`
     x = 10;
     ^~~~~~~

如果你想绑定的变量可变你应该使用**mut:**

    let mut x = 5; // mut x: i32
    x = 10;

默认情况下，变量绑定不可变不是一个单一原因，我们通过rust关注的焦点**安全**来考虑它，如果你忘记使用**mut**,编译器会发现这个问题，
提示你，你可能改变了一些数据，但是你或许不想改变他们，如果绑定默认是可变的，编译器将不能够告诉你你的数据被改变了，如果你打算改变数据，
解决办法很简单，就是加上**mut**

还有一些其他好的理由在适当的时候避开可变变量，但是他们超出了本指南的范围，一般来说，你通常可以避开明显的可变变量，rust中这么多很可取。
这么说吧。有时候变量也是需要的。不应该被禁止。

让我们试试这个，改变这个路径下的源码**src/main.rs**像这样:

    fn main() {
        let x: i32;

        println!("Hello world!");
    }

使用**cargo build**命令编译他们，你会得到一个警告，然是它仍然会打印"Hello World":
 
    Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
    src/main.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variable)] on by default
    src/main.rs:2     let x: i32;
                          ^
Rust警告我们没有使用这个绑定的变量，但是因为我们每使用它。它也没有坏处，也没造成破坏，事情总是变化的。让我们使用这个x，改变你的程序像下面这样

    fn main() {
        let x: i32;

        println!("The value of x is: {}", x);
    }    
                       
尝试编译它，报错了：

    $ cargo build
    Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
    src/main.rs:4:39: 4:40 error: use of possibly uninitialized variable: `x`
    src/main.rs:4     println!("The value of x is: {}", x);
                                                    ^
    note: in expansion of format_args!
    <std macros>:2:23: 2:77 note: expansion site
    <std macros>:1:1: 3:2 note: in expansion of println!
    src/main.rs:4:5: 4:42 note: expansion site
    error: aborting due to previous error
    Could not compile `hello_world`.
      
Rust不允许你使用了一个没初始化的变量，下面我们好好谈论下这个加入到**println!**的大括号.

如果你在你的打印字符串里加入了两个花括号(或者叫moustaches),Rust会解析成想要在这个位置插入某个值，
字符串插入是科学术语，意思就是说“在一个字符中间插入”，字符串后面加一个逗号，然后是变量x，表示x作为我们想插入的值，
如果有多个变量要插入，这个逗号用来分隔我们传递给函数和宏的参数。

当你只使用花括号时，Rust尝试通过检查他的类型用一种语义良好的方式显示，如果你指定一种更详细的方式显示，这有[许多可选项][op]
我们仅使用了默认，对于输出整型并不复杂
[op]: http://doc.rust-lang.org/std/fmt/

[If][if]

[if]: if.md





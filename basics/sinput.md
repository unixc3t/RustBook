[数组，向量，切片][avs.md]

[avs.md]

#标准输入
- - -
从键盘得到输入十分简单,但是也要使用一些我们以前没用过的东西,下面的例子读取输入，然后打印出来:

    fn main() {
        println!("Type something!");
    
        let input = std::old_io::stdin().read_line().ok().expect("Failed to read line");
    
        println!("{}", input);
    }
    
让我们一步一步仔细分析这些代码片段：

    std::old_io::stdin();
    
这里调用了一个函数,**stdin()**,这个函数在**std::old_io**模块中 。
和你想的一样,**std**中的每样东西都是有Rust提供,我们将在稍后的模块系统中讨论,

一直写这种全限定名十分讨厌,我们能使用use语句来导入:

    use std::old_io::stdin;

    stdin();
    
最佳实践是不要导入单个函数，而是导入模块,仅仅使用一级限定

    use std::old_io;

    old_io::stdin();
    
我们使用上面的风格来更新我们的例子:
    
     use std::old_io;

    fn main() {
        println!("Type something!");
    
        let input = old_io::stdin().read_line().ok().expect("Failed to read line");
    
        println!("{}", input);
    }
    
下一个:

    .read_line()
    
这个**read_line()**可以被**stdin()**返回的结果调用,然会一整行输入,简单漂亮

    .ok().expect("Failed to read line");
   
你还记得这个代码么:

    enum OptionalInt {
        Value(i32),
        Missing,
    }
    
    fn main() {
        let x = OptionalInt::Value(5);
        let y = OptionalInt::Missing;
    
        match x {
            OptionalInt::Value(n) => println!("x is {}", n),
            OptionalInt::Missing => println!("x is missing!"),
        }
    
        match y {
            OptionalInt::Value(n) => println!("y is {}", n),
            OptionalInt::Missing => println!("y is missing!"),
        }
    }
    
不管我们是否有值，我们每次都要匹配查看,虽然我们知道x有一个值,但是**match**强制我们去处理**missing**的情况，99%的情况下我们我们想这样，但是有时,我们比编译器更了解，

同样地,**read_line()**没有返回一行输入的数据,或许会返回一行输入的数据，或许会失败不会返回,或许在我们的程序没有运行在终端是发生,或者作为计划任务一部分时发生,或者在没有标准输入的上下文的地方发生，基于这些原因,**read_line**返回一个类似**OptionalInt**类型:一个**IoResult<T>**类型的数A据,我们没有讨论过IoResult<T>，因为OptionalInt的泛型,你可以认为它是一样的东西,使用于任何类型，不仅仅是**i32**

Rust给**IoResult<T>**类型提供了一个方法ok(),和**match**语句做一样的事情,但是假设我们有一个有效的值,我们在返回结果上调用**expect()**方法，如果我们没有有效的值将会警告我们,在这个例子中,如果我们没有得到输入,我们的程序不会工作,使用这个很不错,在大多数情况下，我们想显示的处理错误信息,如果程序崩溃,**except()**方法会给我们一个错误的信息.

在指南的后面我们会详细了解工作细节，现在有这些基本的理解就可以工作了

回到我们的代码，下面是一个更新版

    
    use std::old_io;

    fn main() {
        println!("Type something!");
    
        let input = old_io::stdin().read_line().ok().expect("Failed to read line");
    
        println!("{}", input);
    }
    
这么写很长,Rust允许你灵活的使用空格，我们可以这么写

    use std::old_io;

    fn main() {
        println!("Type something!");
    
        // here, we'll show the types at each step
    
        let input = old_io::stdin() // std::old_io::stdio::StdinReader
                      .read_line() // IoResult<String>
                      .ok() // Option<String>
                      .expect("Failed to read line"); // String
    
        println!("{}", input);
    }
    
或者这样更可读,或者不是，这个你自己选择

这些就是你需要了解的标准输入的全部基本知识,不是很复杂，但是有许多小细节

[猜谜游戏][game]

[game]: game.md


























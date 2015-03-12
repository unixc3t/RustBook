#猜谜游戏
- - -
好的! 我们以我们已经掌握了Rust的基本,让我们写一个大一点程序.

作为我们的第一个项目,我们将实现一个经典的初学者编程问题:猜谜游戏。游戏玩法:我们的程序将会生成一个1到100之间的随机数,然后提示我们输入一个我们猜的数字，根据我们输入的数字,程序会告诉我们输入的太大还是太小，一次就猜对的话，它会出和我们，听懂了?

#建立项目

- - -
让我们创建一个新项目,进入你的项目目录,还记得**hello_world**项目时,我们如何创建目录结构和**Cargo.toml**文件？Cargo有一个命令可以帮助我们, 一个小示例:
    
    $ cd ~/projects
    $ cargo new guessing_game --bin
    $ cd guessing_game
    
我们将项目名字传给**cargo new**,加上 --bin 标记，因为我们是编写可运行程序不是库。

检查我们生成的Cargo.toml文件:

    [package]

    name = "guessing_game"
    version = "0.0.1"
    authors = ["Your Name <you@example.com>"]
    
Cargo从你的系统环境变量得到这些信息,如果不正确，修改正确.

最后，Cargo生成了一个"Hello,World"程序给我们, 查看**src/main.rs**文件

    fn main() {
        println!("Hello, world!")
    }

编译Cargo帮会我生成的程序

    $ cargo build
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
棒极了! 打开**src/main.rs**文件，我们将在这个文件里写我们的代码。我们将在指南后面讨论如何编写多个文件的项目。

在我们开始之前，让我在介绍一个Cargo的命令:run。Cargo run 是一个类似 Cargo build的命令，但它编译后还会执行产生的二进制文件。

    $ cargo run
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
         Running `target/guessing_game`
    Hello, world!

ok!当你的项目需要快速迭代的时候,run命令会很方便。我们的游戏就是这样一个项目,我们每次迭代进入下一步都需要快速测试。

#处理猜测数字

- - -
让我们开始吧，首先我们需要让我们的程序允许选手输入猜测的数字，下面代码加入到**src/main.rs**中:

    use std::old_io;

    fn main() {
        println!("Guess the number!");
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    
        println!("You guessed: {}", input);
    }
    
当我们讨论标准输入的时候。上面的代码你见过,我们使用use导入了**std::old_io**模块，我们的**main**函数包含了我们整个程序逻辑, 我们打印了宣布游戏开始信息,告诉玩家输入一个猜测的数字,得到玩家输入的数字,然后打印它

因为我们讨论过了标准输入，这里我不会详细讲解这个。如果你需要复习，请回头看上一节

#生成猜谜数字

下一步,我们生成一个猜谜数字,实现这个，我们需要Rust的随机数生成器,我们前面没有说过这个。在标准库中，Rust包含了一组有意思的函数,如果你需要一些代码,或许已经给你写好了，我们知道Rust有随机数生成器,但是我们不知道怎么使用它。

打开文档。Rust有一篇关于标准库的文档。你可以点击[链接][doc]查看,文档里有详细信息,最好的地方就是搜索栏。在顶部,有一个你可以输入要搜索的词搜索栏。这个搜索栏十分简单,但是用起来很方便。如果你输入"random"在搜索栏中,页面就会变成[这样]
[this],第一个链接结果是[**std::rand::random**][srr],如果我们点击它，就会跳转到它的文档页。

页面中告诉我们一些信息：函数的类型签名,一些说明信息和一些例子。让我们修改我们的代码，加入随机函数:

    use std::old_io;
    use std::rand;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = (rand::random() % 100) + 1; // secret_number: i32
    
        println!("The secret number is: {}", secret_number);
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    
    
        println!("You guessed: {}", input);
    }
    

第一个要改变的是加入**use std::rand**，文档中已经说明了。然后加入了一个**let**表达式创建了一个变量,名字叫做**secret_number**,然后打印出来。

你或许想知道为什么在**rand::random()**的返回结果后面使用**%**，这个操作符叫*取模运算符*，它返回除法的余数。**rand::random()**的返回结果调用取模运算符，目的限定值在0-99之间。我们给结果值加1，确保值范围是1-100.使用取模运算符会产生一些误差，但是对这个例子来说，不是很重要。

让我们使用cargo build编译一下:

    $ cargo build
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
    src/main.rs:7:26: 7:34 error: the type of this value must be known in this context
    src/main.rs:7     let secret_number = (rand::random() % 100) + 1;
                                           ^~~~~~~~
    error: aborting due to previous error
    
程序不工作!提示信息说“值类型必须在上下文中”,这是什么问题?因为,**rand::random()**可以生成很多种随机数,不仅仅是整型.
在这个例子中,Rust不确定**random()**生成的值类型。所以我不得不帮助它。使用数字字面值，我们仅仅是加了**i32**到返回结果上告诉Rust是整型,并不影响函数功能。有一点不一样，看下面代码:

    rand::random::<i32>();
    
代码意思是,请给我一个随机的i32类型的值.我们将代码改成这样:

    use std::old_io;
    use std::rand;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = (rand::random::<i32>() % 100) + 1;
    
        println!("The secret number is: {}", secret_number);
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    
    
        println!("You guessed: {}", input);
    }
    
然后运行我们的程序几次:

    $ cargo run
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
         Running `target/guessing_game`
    Guess the number!
    The secret number is: 7
    Please input your guess.
    4
    You guessed: 4
    $ ./target/guessing_game
    Guess the number!
    The secret number is: 83
    Please input your guess.
    5
    You guessed: 5
    $ ./target/guessing_game
    Guess the number!
    The secret number is: -29
    Please input your guess.
    42
    You guessed: 42

等等.负29？我们想要的是1到100之间的数.我们有两个选择:我们要么告诉**random()**生成一个无符号的整数，得到一个正数.要么使用**abs()**函数。我们使用无符号整数方案。如果我们想要一个随机正数，我们就应该要求随机随机正数.我们的代码像下面这样:

    use std::old_io;
    use std::rand;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = (rand::random::<u32>() % 100) + 1;
    
        println!("The secret number is: {}", secret_number);
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    
    
        println!("You guessed: {}", input);
    }
    
测试一下：
    
    $ cargo run
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
         Running `target/guessing_game`
    Guess the number!
    The secret number is: 57
    Please input your guess.
    3
    You guessed: 3
    
好极了！让我们比较一下我们猜的和生成的:


#比较猜测

- - -
如果你还记得指南前面我们使用了一个**cmp**函数来比较两个数。让我们加入进去，同时使用match语句来比较我们的猜测数字和生成的数字

    use std::old_io;
    use std::rand;
    use std::cmp::Ordering;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = (rand::random::<u32>() % 100) + 1;
    
        println!("The secret number is: {}", secret_number);
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    

        println!("You guessed: {}", input);
    
        match cmp(input, secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
        

}

fn cmp(a: i32, b: i32) -> Ordering {
    if a < b { Ordering::Less }
    else if a > b { Ordering::Greater }
    else { Ordering::Equal }
}


我们尝试编译一下，得到些一些错误:

    $ cargo build
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
    src/main.rs:20:15: 20:20 error: mismatched types: expected `i32` but found `collections::string::String` (expected i32 but found struct collections::string::String)
    src/main.rs:20     match cmp(input, secret_number) {
                                 ^~~~~
    src/main.rs:20:22: 20:35 error: mismatched types: expected `i32` but found `u32` (expected i32 but found u32)
    src/main.rs:20     match cmp(input, secret_number) {
                                        ^~~~~~~~~~~~~
    error: aborting due to 2 previous errors
    
这种事写程序时会经常发生,这也是Rust的优势之一。你应该经测试一些代码。编译看看,Rust会告诉你什么地方出错了。在这个例子中，我们的**cmp**函数比较有符号整型,但是我们传递给他无符号整型,在这个例子中，修正错误很简单，我们重写**cmp**函数。修改传递参数为**u32**类型：

    use std::old_io;
    use std::rand;
    use std::cmp::Ordering;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = (rand::random::<u32>() % 100) + 1;
    
        println!("The secret number is: {}", secret_number);
    
        println!("Please input your guess.");
    
        let input = old_io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
    
    
        println!("You guessed: {}", input);
    
        match cmp(input, secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }



    fn cmp(a: u32, b: u32) -> Ordering {
        if a < b { Ordering::Less }
        else if a > b { Ordering::Greater }
        else { Ordering::Equal }
    }
    
然后再尝试编译:
    
    $ cargo build
       Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
    src/main.rs:20:15: 20:20 error: mismatched types: expected `u32` but found `collections::string::String` (expected u32 but found struct collections::string::String)
    src/main.rs:20     match cmp(input, secret_number) {
                                 ^~~~~
    error: aborting due to previous error
    

这个错误和尚一个类似:我们希望得到一个u32类型，但是却得到一个String类型。那是因为我们输入变量来自标准输入。你能输入任何你猜的值,试试:
    
    $ ./target/guessing_game
    Guess the number!
    The secret number is: 73
    Please input your guess.
    hello
    You guessed: hello
    
你注意到了我们可以运行我们的程序及时它编译错误了。可以运行是因为上一个编译版本还在。要小心这点！

我们得到一个String类型，但是我们需要u32类型。我们该怎么做？有一个函数可以做到:

    let input = old_io::stdin().read_line()
                           .ok()
                       .expect("Failed to read line");
    let input_num: Result<u32, _> = input.parse();
    

































[srr]: http://doc.rust-lang.org/std/rand/fn.random.html
[this]: http://doc.rust-lang.org/std/?search=random
[doc]: http://doc.rust-lang.org/std/


































































[匹配][match]

[match]: match.md

#循环
- - -
循环是我们没有学习的最后一个基本结构,Rust有两种主要的循环结构:for和while

###for
- - -
for循环用于循环特定的次数,Rust的for循环和其他系统级语言有一点不一样,但是，Rust的循环看起来不是c语言的风格：
    
    for (x = 0; x < 10; x++) {
        printf( "%d\n", x );
    }
相反，看起来是这个样子：
    
    for x in 0..10 {
        println!("{}", x); // x: i32
    }

抽象结构如下:
    
    for var in expression {
        code
    }

这个expression是一个迭代器,关于迭代器我们稍后在指南中详细介绍.迭代器返回一些元素,每个元素消耗一次迭代，返回值绑定名字为var的变量,返回值对于循环体有用的，一旦循环体结束,下一个值就会由迭代器返回，然后进行另一次循环，直到迭代器不返回值，循环结束。

在我们的例子中,0..10是一个表达式,包括一个开始位置和一个结束位置,产生一个迭代器遍历这些值,表达式的上限是被排除的(不包括结束位置)，我们代码会打印0到9，不包含10.

Rust有意没有使用c语言风格的循环。手动控制循环的每个元素复杂又容易出错，即使对于C语言专家来说。

当在指南后面涉及到迭代器时。我们会更详细介绍for.

###while
- - -
Rust中的另一个循环结构就是while,看起来像下面这样:

    let mut x = 5; // mut x: u32
    let mut done = false; // mut done: bool
    
    while !done {
        x += x - 3;
        println!("{}", x);
        if x % 5 == 0 { done = true; }
    }
    
while正是这种你不确定要循环多少次时，需要使用的循环。

如果你想要一个无限循环，你或许要像下面这样写:
    
    while true{
    
但是，Rust有一个专用的关键字,来做这件事:
    
    loop {
    
Rust的控制流处理器处理loop结构相比于一个while true有一些不同，因为我们知道它会一直循环下去,理解其中细节不是十分重要，但总的来说,我们给编译器越多的信息，在代码生成和安全方面做得越好，所以当你打算使用无限循环的时候请使用loop

###提前退出迭代

- - -
让我们看一下之前的代码:
    
    let mut x = 5;
    let mut done = false;

    while !done {
        x += x - 3;
        println!("{}", x);
        if x % 5 == 0 { done = true; }
    }
    
我们保留了一个可变的布尔型变量done，来判断什么时候退出循环,Rust有两个关键字帮助我们修改迭代:**break和continue**

下面的例子中，我们使用break来优化这个循环:
    
    let mut x = 5;
    
    loop {
        x += x - 3;
        println!("{}", x);
        if x % 5 == 0 { break; }
    }
    
我们使用**loop**进行无限循环并且使用**break**提早结束循环

**continue**与break相似，但是，它不是结束循环，进入下一次循环 ，下面的程序打印奇数:

    for x in 0u32..10 {
        if x % 2 == 0 { continue; }
    
        println!("{}", x);
    }
    
continue 和 break 都可以在两种循环中使用。


[字符串][str]

[str]: str.md













    



























  
  

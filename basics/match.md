#匹配(match)
- - - 
常常遇到一个简单的**if/else**结构不能满足使用，因为你会有超过两种可能的选项，else分支会变得很复杂。如何解决呢？

Rust有一个关键字,**match**，它可以替代**if/else**的复杂结构并提供强大的功能,看下面代码:
    
    let x = 5;

    match x {
        1 => println!("one"),
        2 => println!("two"),
        3 => println!("three"),
        4 => println!("four"),
        5 => println!("five"),
        _ => println!("something else"),
    }

**match** 带一个表达式并且基于这个表达式值的分支,分支的每个arm的形式如 val => expressin，当有值匹配到了，arm的表达式将会被评估,之所以叫**match**因为"模式匹配"是一个术语,**match**是他的一个实现

那么这个高级在哪里呢?我们来看一下，首先，**match**强调有穷性检查,注意最后一个arm,那个用下划线的,如果我们移除这条，我们会得到一个错误:
    
    error: non-exhaustive patterns: `_` not covered
    
换句话说，Rust尝试提醒我们忘记一个值,因为x是一个整型，Rust知道它可能是许多值，例如，6.没有\_就没有值可以匹配，所以Rust拒绝编译,
这个\_就像”catch-all arm“，如果没有任何一个arm匹配到，使用"\_"的arm将匹配，因为我们有这个”catch-all arm“,我们现在有一个可以匹配任何x的值的arm，所以我们的程序可以编译成功.

match语句也使用枚举，还记得我们前面的枚举章节的代码:

    use std::cmp::Ordering;

    fn cmp(a: i32, b: i32) -> Ordering {
        if a < b { Ordering::Less }
        else if a > b { Ordering::Greater }
        else { Ordering::Equal }
    }
    
    fn main() {
        let x = 5;
        let y = 10;
    
        let ordering = cmp(x, y);
    
        if ordering == Ordering::Less {
            println!("less");
        } else if ordering == Ordering::Greater {
            println!("greater");
        } else if ordering == Ordering::Equal {
            println!("equal");
        }
    }

使用match重写

    use std::cmp::Ordering;

    fn cmp(a: i32, b: i32) -> Ordering {
        if a < b { Ordering::Less }
        else if a > b { Ordering::Greater }
        else { Ordering::Equal }
    }
    
    fn main() {
        let x = 5;
        let y = 10;
    
        match cmp(x, y) {
            Ordering::Less => println!("less"),
            Ordering::Greater => println!("greater"),
            Ordering::Equal => println!("equal"),
        }
    }

这个版本看起来清爽多了，并且检查详细以确保覆盖了所有Ordering可能性,如果使用**if/else**，例如，我们会我忘记Greater,我们的程序会很好的编译，但是我们在match使用时,忘了这个，我们的程序将不会编译,Rust帮助我们确保覆盖到我们的所有部分。

match表达式也允许我们得到来自枚举中的值,也被称为解构
    
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
    
这就是你怎样得到和使用枚举中的值,也允许我们处理错误和异常计算，例如，一个函数不能确保返回一个计算结果(一个整数)
,但是能够返回一个可选的整型，我们可以使用match处理，正如你看到的,match和枚举在一起十分有用.


match也是一个表达式,意味着我们可以在let绑定的右半部分用它，或者在某些地方直接使用它，我们可以实现将前面的代码像下面这样实现：

    use std::cmp::Ordering;
    
    fn cmp(a: i32, b: i32) -> Ordering {
        if a < b { Ordering::Less }
        else if a > b { Ordering::Greater }
        else { Ordering::Equal }
    }
    
    fn main() {
        let x = 5;
        let y = 10;
    
        println!("{}", match cmp(x, y) {
            Ordering::Less => "less",
            Ordering::Greater => "greater",
            Ordering::Equal => "equal",
        });
    }
    
[循环][loop]

[loop]: loop.md



#复合数据类型
 - - -
 Rust和许多编程语言一样。有很多不同的内置数据类型。已经使用整型类型和字符串类型写了一些例子，下一步，让我们学习一些更复杂存储数据的方式
 
 
 #元组
 
---
第一个我们谈论的数据类型就是元组，元组是一个有固定大小的有序列表。像这样


    let x = (1,"hello")

这是一个由两个圆括号和逗号组成的长度是2的元组，下面是一样的代码。但是有类型注释

    let x:(i32,&str) = (1,"hello");

正如你所看到的，一个元组的类型看起来就是元组(类似字符串的类型就是字符串)，但是每个成员都有自己的类型名不仅仅是有值，细心的读者或许注意到了元组是异构类型:在元组中有一个**i32**和一个**&str**类型，刚刚你简略的看到**&str**作为类型的使用，稍后我们会详细介绍strings类型，Rust中的strings相比在其他语言中复杂一点,目前为止，把**&str**读作*字符串切片*，稍后会学习它。

你可以使用let绑定来访问元组的元素。就像下面:

    let (x,y,z)=(1,2,3);
    println!("x is {}",x);
 
还记得前面我说过，一个let语句的左部分比仅仅用来绑定分配更有用，在这我们就看到。我们可以在let左边放一个模式，如果匹配了右边，我们就可以一次分配绑定多个值，在这个例子里，let析构或打破了元组，分配绑定了3个值
    
模式匹非常强大，稍后我们会多次看到他
   
你也可以将元组作为一个整体来使用,不需要析构，如果元组的类型结构和元素数量一样，你可以分配一个元组给另一个元组，元组长度也一样同时元素类型也一样
    
    let mut x = (1, 2); // x: (i32, i32)
    let y = (2, 3); // y: (i32, i32)

    x = y;
    
也可以使用 "==" 号 判断相当，如果元组有一样类型，将只会编译

    
    let x = (1, 2, 3);
    let y = (2, 2, 4);

    if x == y {
        println!("yes");
    } else {
        println!("no");
    }
    
将会打印no，因为元组里的一些值不相等。

注意，值得顺序也是检查是否相等的因素，下面例子也是打印no

    let x = (1, 2, 3);
    let y = (2, 1, 3);

    if x == y {
        println!("yes");
    } else {
        println!("no");
    }
    
元组的另一个作用就是在函数中返回多个值

    fn next_two(x: i32) -> (i32, i32) { (x + 1, x + 2) }

    fn main() {
        let (x, y) = next_two(5);
        println!("x, y = {}, {}", x, y);
    }
    
虽然Rust的函数只能返回一个值,一个元组也是一个值，用多个值组成的元组，同时，你也可以看到在这个例子中如何析构匹配函数返回的值。

函数是一个非常简单的数据结构，或许不能满足你使用，让我们看看他的兄弟类型，结构体


#结构体
- - -
就像元组，机构体是另一种记录类型，不同的是:结构体给他每个包含的元素一个名字,叫属性或者成员，看下面代码

    struct Point {
        x: i32,
        y: i32,
    }

    fn main() {
        let origin = Point { x: 0, y: 0 }; // origin: Point
    
        println!("The origin is at ({}, {})", origin.x, origin.y);
    }
    
这都什么意思。让我们慢慢讲解，我们使用**struct**关键字来描述一个结构体，起一个名字，通常以一个大写字母开头。使用驼峰命名法,例如:
** PointInSpace, 而不是Point_In_Space.**

我们是用let创建一个结构体实例变量，我们使用key:value的语法来设置每个属性，设置的顺序不需要和描述时的一样。

最后，因为属性有自己名字，我们可以使用.符号来访问属性:origin.x。

默认结构体的值是不可变的，和Rust中的其他绑定一样，使用**mut**使属性可变:

    struct Point {
        x: i32,
        y: i32,
    }
    
    fn main() {
        let mut point = Point { x: 0, y: 0 };
    
        point.x = 5;
    
        println!("The point is at ({}, {})", point.x, point.y);
    }
打印 **The point is at (5,0)**


#元组结构体和新类型
- - -
Rust有另一个数据类型就像元组和结构体的混合组成的，叫做元组结构体,元组结构体有名字。他们的属性没有:
    
    struct Color(i32, i32, i32);
    struct Point(i32, i32, i32);
    
上面两个元组结构体不相等，即使有一样的值
    
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
    
一般来说，使用结构体比使用元组结构体好，我们可以将Color和Point用结构体替代
    
    struct Color {
        red: i32,
        blue: i32,
        green: i32,
    }

    struct Point {
        x: i32,
        y: i32,
        z: i32,
    }

现在我们有了名字而不是位置， 有意义的名字很重要，使用结构体我们就有了名字

有一种情况，元组结构体十分有用，那就是元组结构体有一个元素。我们管这个叫新类型，因为它允许你创见一个相似的新类型

    struct Inches(i32);

    let length = Inches(10);
    
    let Inches(integer_length) = length;
    println!("length is {} inches", integer_length);
 
正如你看到的，和我们前面在元组那节讨论的一样，你可以通过Let析构提取内部的整型类型,在这个例子中，
**let Inches(integer_length)** 将10分配给**integer_length**
integer_length


#枚举
- - -
最后，Rust有一个"聚合类型",叫做枚举，枚举是Rust中及其有用的特性，它的使用惯穿了整个标准库，枚举类型就是对于一个指定的名字有一组可替代的值
例如，下面我们定义的**Character**可以是一个Digit或者其他什么值，可以使用变量的全限定名字:**Character::Other**，关于::我们后面讲
    
    enum Character {
        Digit(i32),
        Other,
    }

一个枚举体定义和大多数普通类型一样，下面列出了一些示例类型，这些类型也允许在枚举中出现

    struct Empty;
    struct Color(i32, i32, i32);
    struct Length(i32);
    struct Status { Health: i32, Mana: i32, Attack: i32, Defense: i32 }
    struct HeightDatabase(Vec<i32>);

如你所看到的基于这些子数据结构，枚举体类似结构体，可以存储也可以不存储数据，例如**Character**中，**Digit**是一个与i32相关联的名字，other只是一个名字，然而，他们的不同之处十分有用。

与结构体一样，枚举没有默认的访问操作符，例如比较操作符(==和!=),二进制操作符(*和+)逻辑操作符(>和>=) 。使用前面的Character 为示例。下面代码报错:
    
    // These assignments both succeed
    let ten  = Character::Digit(10);
    let four = Character::Digit(4);
    
    // Error: `*` is not implemented for type `Character`
    let forty = ten * four;
    
    // Error: `<=` is not implemented for type `Character`
    let four_is_smaller = four <= ten;
    
    // Error: `==` is not implemented for type `Character`
    let four_equals_ten = four == ten;
    
 这看起来是相当受限制，但是这点限制我们可以克服，有两种方式，我们自己实现这些操作符，或者使用[match][match]关键字，
 我们对Rust实现的等号没有足够了解，但是我们可以使用来自标准库的**Ordering**枚举类型
 
     enum Ordering {
        Less,
        Equal,
        Greater,
    }
 
 因为wome我们没有定义**Ordering**, 我们必须从标准库中使用use关键字导入,下面是一个怎样使用Ordering的例子:
     use std::cmp::Ordering;

    fn cmp(a: i32, b: i32) -> Ordering {
        if a < b { Ordering::Less }
        else if a > b { Ordering::Greater }
        else { Ordering::Equal }
    }
    
    fn main() {
        let x = 5;
        let y = 10;
    
        let ordering = cmp(x, y); // ordering: Ordering
    
        if ordering == Ordering::Less {
            println!("less");
        } else if ordering == Ordering::Greater {
            println!("greater");
        } else if ordering == Ordering::Equal {
            println!("equal");
        }
    }
     

这个 :: 符号用来指明一个命名空间,在这个例子中,Ordering位于std模块的子模块cmp中，指南的后面我们会详细学习模块，现在，你需要知道你所能够使用use从标准库导入你需要的东西.

让我们讨论一下例子中的代码，cmp是一个比较两个数的函数。返回一个Ordering类型,如果a小于b 我们返回Ordering::Less, 如果a大于b Ordering::Greater, 或者两个数相等 Ordering::Equal,注意。枚举的每个变量都在枚举命名空间后面，是 Ordering::Greater 不是 Greater.

ordering变量的类型是Ordering，值是三个值的其中一个，我们使用一组if/else语句来对比是哪个值。

Ordering::Greater太长了，我们使用枚举体来替代，如下

    use std::cmp::Ordering::{self, Equal, Less, Greater};

    fn cmp(a: i32, b: i32) -> Ordering {
        if a < b { Less }
        else if a > b { Greater }
        else { Equal }
    }
    
    fn main() {
        let x = 5;
        let y = 10;
    
        let ordering = cmp(x, y); // ordering: Ordering
    
        if ordering == Less { println!("less"); }
        else if ordering == Greater { println!("greater"); }
        else if ordering == Equal { println!("equal"); }
    }
    
使用变体形式更简洁，但是容易引起命名冲突，这么做的时候要谨慎，如果导入的变量很少这个是一个好的风格.

如你所看到的。枚举是一种很强大的数据表现形式,当他们是泛型跨类型时候甚至更有用，尽管我们还没有学习泛型，让我们来学习怎么使用它们来进行模式匹配，让我们可以以一种优雅的方式来析构聚合类型(枚举的理论术语)而避免使用看起来可读性差的**if/else语句**
 
 [match]: match.md
    
    



























    
    
    
    

































    
    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    




    
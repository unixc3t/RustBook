[If][if]

[if]: if.md

#函数
- - -

目前你已经看到一个函数了，那就是**main**函数:
    
    fn main() {
    
    }
    
这可能是最简单的函数声明了，就像我们以前说的，**fn**表示这是一个函数，后面跟着一个函数名，两个圆括号,函数没有参数，两个大的花括号里面是函数体.下面是一个叫foo的函数:

    fn foo() {
        
    }
    

如何接受参数？ 下面这个函数打印了一个数字

    fn print_number(x:i32) {
        println!("x is : {}",x);
    }

下面是一个较完整的程序用来打印数字

    fn main() {
        print_num(5);
    }
    
    fn print_num(x: i32) {
        println!("x is:{}",x);
    }

正如你看到的，函数参数使用类似**let**声明,在参数名冒号后面加一个类型标识。

下面是一个打印两个数相加之和的程序:


    fn main() {
        print_sum(5, 6);
    }

    fn print_sum(x: i32, y: i32) {
        println!("sum is: {}", x + y);
    }
 
 调用函数或者声明函数时，使用逗号分隔两个函数参数，
 
 和let不一样，你必须标识函数参数类型， 如果不声明，就会报错
     
     fn print_sum(x, y) {
        println!("sum is: {}", x + y);
    }
 
下面错误:
    
    hello.rs:5:18: 5:19 expected one of `!`, `:`, or `@`, found `)`
    hello.rs:5 fn print_number(x, y) {

这是一个故意的设计，对于全推演语言（比如 Haskell），最佳实践也建议写上类型的，我们赞同函数声明是指定参数类型和在函数体内进行类型推断，实在完全类型推断和完全不推断之间一个好的平衡点。

那返回值呢？ 下面的函数给一个数值加1:
    
    fn add_one(x: i32) -> i32 {
        x + 1
    }
   
 
Rust函数恰好返回一个值, 在一个箭头后面声明类型 箭头有一个中划线"-"和一个大于号">"组成
    
注意这没有分号,如果我们加上分号:

    
    fn add_one(x: i32) -> i32 {
        x + 1;
    }
   
运行后报错:

    error: not all control paths return a value
    fn add_one(x: i32) -> i32 {
         x + 1;
    }

    help: consider removing this semicolon:
         x + 1;
             ^

还记得我们前面讨论过的分号和单元类型么？我们的函数要求返回一个i32类型，但是后面有了分号。就返回了()而不是i32类型，Rust分析这个可能不是我们想要的，所有建议我们移除分号。

这个非常像前面的if语句，{}里的结果就是表达式返回的值，其他面向表达式的语言，例如ruby的运行和这个很像,但是在系统级编程语言里就很不常见，当人们第一次了解这个的时候，通常认为这是一个bug。但是Rust的类型系统十分强大，单元是它自己的特殊类型，我们没看到一个错误提交说在返回位置加入或移除一个分号引起的bug。

但是提前返回呢？ Rust有一个关键字**return**:

    fn foo(x: i32) -> i32 {
        if x < 5 { return x; }

        x + 1
    }

函数最后一行使用return返回。但是不太推荐

    fn foo(x: i32) -> i32 {
        if x < 5 { return x; }

        return x + 1;
    }
        

如果你没有使用过面向表达式语言的经验，按照前面那样不使用return可能觉得有些担心，但是随着用久了就会习惯，如果是生产环境代码我们不会那么写，

    fn foo(x: i32) -> i32 {
        if x < 5 {
            x
        } else {
            x + 1
        }
    }
    
 
因为if是一个表达式，在这个函数里唯一的表达式， if表达式的结果作为返回值。


#分叉函数

- - - 

Rust有一个特殊语法适用于“分叉函数”,就是没有返回值:

    fn diverges() -> ! {
        panic!("This function never returns!");
    }
    
panic!是一个宏，类似我们前面看到的println!()，和println!()不一样的地方是，painc!()造成当前执行线程崩溃并打印给定的信息

因为这个函数会引起程序崩溃，永远不会返回值，所以返回类型是'!',读作"分叉",一个分叉函数可以用于任何类型
    
    let x: i32 = diverges();
    let x: String = diverges();
    
  我们没有很好的使用分叉函数，因为它需要和rust的其他特性联合起来使用，不过过后你看到 ** -> !**时，你就会知道他为什么叫 这个
  
  [注释][commnet]
  
  [commnet]: http://doc.rust-lang.org/book/comments.html
    
    
    
    
    
    
    
    

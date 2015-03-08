[循环][loop]

[loop]: loop.md
#字符串
- - - 
字符串是任何程序员都需要精通的一项重要内容,因为Rust关注的重点是系统,所以Rust的处理字符串和其他语言有一些不同，在任何时候你有一个可变大小的数据结构，事情就会变得棘手,字符串是一个大小可变的结构，也就是说,Rust的字符串工作方式和其他语言里的不一样，例如c语言.

让我们深入了解一下,一个字符串就是Unicode标量以UTF-8编码方式编码的字节序列，所有的字符串被保证可以有效的被编码为UTF-8序列,此外，字符串不是以null结尾的，并且可以包含null字节

Rust有两个主要字符串类型：**&str**和**String**。

首先是&str，被叫做字符串切片，字符串字面量属于&str类型:

    let string = "Hello there."; // string: &str
    
上面的字符串是静态分配的,并且它被保存在我们编译后的程序中，整个程序运行中它一直存在, **string**的绑定是指一个引用指向静态分配的字符串.字符串切片有固定的大小，并且不可变，

String类型,换种说法就是，堆分配字符串,这类型字符串是可变的,并且保证是UTF-8编码，**String**类型字符串通常是由字符串切片调用**to_string**方法转换而来

    let mut s = "Hello".to_string(); // mut s: String
    println!("{}", s);
    
    s.push_str(", world.");
    println!("{}", s);

使用一个&符号可以将Strings类型强制转换为&str类型

    fn takes_slice(slice: &str) {
        println!("Got: {}", slice);
    }
    
    fn main() {
        let s = "Hello".to_string();
        takes_slice(&s);
    }

由上可见,一个String类型转换成&str类型很廉价,但是&str转换到String类型涉及到分配内存,除非你不得已否则不要这么做。

这就是Rust中最基本的字符串。他们或许比你使用过脚本语言复杂一点，当遇到底层细节的时候，他们就很关键，记住**String**类型分配内存控制它的数据,
**&str**是一个指向另一个字符串的引用,知道这些就可以了

[Arrays,Vector,Slices][comp]

[comp]: avs.md


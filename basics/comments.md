[函数][fun]

[fun]: fun.md

#注释

- - -

现在我们已经对函数有了了解，是时候学习注释了，注释就是帮助其他程序员理解你的代码，编译器通常会忽略他们。

Rust有两种注释，行注释和文档注释。

    // 行注释就是在 ”//“ 后面并且一直到延伸行末尾  
    let x = 5; //这就是一个行注释
    // 如果你的注释比较长，你可以让他们紧挨着
    // “//”和注释之间有一个空格
    // 可能性好一些
    
另一种注释叫做文档注释。文档注释使用///代替//，并且支持markdown标记

    
    /// `hello` is a function that prints a greeting that is personalized based on
    /// the name given.
    ///
    /// # Arguments
    ///
    /// * `name` - The name of the person you'd like to greet.
    ///
    /// # Example
    ///
    /// ```rust
    /// let name = "Steve";
    /// hello(name); // prints "Hello, Steve!"
    /// ```
    fn hello(name: &str) {
        println!("Hello, {}!", name);
    }
    
当你写文档注释的时候，可以给参数返回值分节解释和添加一些例子，这会有利于理解，不要担心**&str**这个符号，我们稍后会学习

你也可以使用rustdoc工具将文档注释转换成html形式的文档
    
[复合数据类型][compound]

[compound]: http://doc.rust-lang.org/book/compound-data-types.html
#测试
- - -
    程序测试是一项非常有效的展现bug的方式，但是要证明他们不存在就显得无望和不足
                                                  -----Edsger W. Dijkstra "谦逊的程序员"
                                                  
  让我们讨论一下如何测试Rust代码。我们不会讨论正确的测试Rust代码的方式。有许多正确的和错误的测试方式。这些
  方法都是用一样的工具，我们将展示使用他们的语法。
  
  #测试属性
   - - -
它很简单，Rust中的一个测试程序就是标明test属性的函数，使用Cargo新建一个项目，叫做adder：
    $ cargo new adder
    $ cd adder
    
当你新建一个项目的时候cargo会自动生成一个简单的测试，在src/lib.rs中:
    
    #[test]
    fn it_works() {
    }
    
注意**#[test]**,这个属性表示这是一个测试函数.他没有函数体，可以通过。我们使用**cargo test**运行测试程序

    $ cargo test
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 0 tests
    
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
   
cargo编译运行我们的测试程序。这有两组输出。一个是我们写的测试。另一个是文档测试，我们稍后讨论这个，现在我们看这句。

    test it_works ... ok

注意这个**it_works**,这是我们的函数名:
    
    fn it_works() {
    
我们还得到一行摘要:

    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured

为什么我们什么都没做测试就通过了，没有**panic!**的测试通过。有**panic!**的测试失败。让我们使测试失败:
    
    #[test]
    fn it_works() {
        assert!(false);
    }
assert!是Rust提供的一个宏,接受一个参数，如果参数是true，什么都不发生，如果参数是false,它panic!.让我们运行我们的程序:
    
    $ cargo test
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test it_works ... FAILED
    
    failures:
    
    ---- it_works stdout ----
            thread 'it_works' panicked at 'assertion failed: false', /home/steve/tmp/adder/src/lib.rs:3
    
    
    
        failures:
        it_works
    
    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured
    
    thread '<main>' panicked at 'Some tests failed', /home/steve/src/rust/src/libtest/lib.rs:247
    
Rust指示我们的测试失败
        
    test it_works ... FAILED
    
并且摘要上可以看到:
    
    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured
        
我们也得到一个非0的状态码:
    
    $ echo $?
    101

这个很有用，如果你想集成cargo test到其他工具里.
我们可以使用属性**shuld_panic** 反转我们测试程序的失败结果

    #[test]
    #[should_panic]
    fn it_works() {
        assert!(false);
    }
    
如果我们panic!了。测试将会成功，如果我们完成了测试将会失败 让我们试试:
    
    $ cargo test
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 0 tests
    
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
    
Rust提供了另一个宏,**assert_eq!** 用于比较两个参数是否相等

    #[test]
    #[should_panic]
    fn it_works() {
        assert_eq!("Hello", "world");
    }
    
这个测试通过还是失败?因为使用了shuld_panic属性，所以他通过了

    $ cargo test
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 0 tests
    
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
    
shuld_panic测试是脆弱的，，它很难保证一个未知原因导致的失败通过。为了解决这个问题。一个可选的**expected**参数可以
加入到should_panic属性中，这个测试属性会确保我们的错误信息包含我们提供的文本，一个安全版本的例子如下:
    
    #[test]
    #[should_panic(expected = "assertion failed")]
    fn it_works() {
        assert_eq!("Hello", "world");
    }
    

这就是所有的基础。让我们写一个真正的测试:

    pub fn add_two(a: i32) -> i32 {
        a + 2
    }

    #[test]
    fn it_works() {
        assert_eq!(4, add_two(2));
    }
    
这时一个非常普通的使用assert_eq!的例子，调用函数传入已知的参数和预期的输出做比较
    
#The test module
- - -
我们的例子的测试方式并不是习惯用法。它缺少测试模块， 下面的写法是惯用法:

    pub fn add_two(a: i32) -> i32 {
        a + 2
    }
    
    #[cfg(test)]
    mod test {
        use super::add_two;
    
        #[test]
        fn it_works() {
            assert_eq!(4, add_two(2));
        }
    }

代码有一些变化,首先就是引入了一个使用cfg属性的mod test。 这个模块允许我们集合所有的测试，并且如果需要的话定义辅助函数,
它不会成为我们crate的一部分。当我们运行这些测试的时候，这个cfg属性表示仅编译我们的测试代码，它可以节省编译时间，并且确保我们的测试代码不会纳入构建之中
    

第二个变化就是。使用了use声明，因为我们在写一个内部模块，我们需要将我们的测试函数引入到当前作用域内，如果你有一个大模块，这样多很烦人。这有一个glob特性的使用方法，像下面这样，改变我们的代码:
    
    pub fn add_two(a: i32) -> i32 {
        a + 2
    }
    
    #[cfg(test)]
    mod test {
        use super::*;
    
        #[test]
        fn it_works() {
            assert_eq!(4, add_two(2));
        }
    }
    
注意使用use关键字那行，让我们运行测试

    $ cargo test
        Updating registry `https://github.com/rust-lang/crates.io-index`
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test test::it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 0 tests
    
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
   
当前使用 test module的模式存放你的“单元风格”的测试，仅仅是测试一个小功能可以这么做。但是"集成风格"的测试呢？对于这个。我们有测试目录

#the test directory

- - -
为了写一个集成测试，我们新建立一个tests目录，里面放一个test/lib.rs文件，写上下面的内容:

    extern crate adder;

    #[test]
    fn it_works() {
        assert_eq!(4, adder::add_two(2));
    }
    
代码看起来和我们前面看起来一样。但是有些不同,在代码第一行，有extern crate adder 。这时因为测试在tests目录中，不同于crate，需要导入我们的library.这也是为什么tests是写集成测试合适的地方，他们使用library，就像其他用户使用一样。

让我们运行代码:

        $ cargo test
       Compiling adder v0.0.1 (file:///home/you/projects/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test test::it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
         Running target/lib-c18e7d3494509e74

    running 1 test
    test it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 0 tests
    
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
    
现在我们将了3部分了。我们前面的测试，加上新的这个

这就是tests directory， test module在这不需要，因为整个测试都集中在这

让我们最后看看第三部分: Documentation tests。

#Documentation tests

- - -
没有什么比带例子的文档更好了。没有什么比不能运行的例子更糟了，因为文档写完代码已经改变，Rust支持自动运行文档中的例子。

这有一份详实的src/lib.rs的例子

    //! The `adder` crate provides functions that add numbers to other numbers.
    //!
    //! # Examples
    //!
    //! ```
    //! assert_eq!(4, adder::add_two(2));
    //! ```
    
    /// This function adds two to its argument.
    ///
    /// # Examples
    ///
    /// ```
    /// use adder::add_two;
    ///
    /// assert_eq!(4, add_two(2));
    /// ```
    pub fn add_two(a: i32) -> i32 {
        a + 2
    }
    
    #[cfg(test)]
    mod tests {
        use super::*;
        
        #[test]
        fn it_works() {
            assert_eq!(4, add_two(2));
        }
    }
  
注意模块的文档使用//! ，函数的文档使用///  Rust的文档支持markdown，所以它支持3个反单引号来标记代码块。上面例子那样，通常包含# Example的部分，紧跟着就是例子
    
让我们再次运行这个测试:

    $ cargo test
       Compiling adder v0.0.1 (file:///home/steve/tmp/adder)
         Running target/adder-91b3e234d4ed382a
    
    running 1 test
    test test::it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
         Running target/lib-c18e7d3494509e74
    
    running 1 test
    test it_works ... ok
    
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
    
       Doc-tests adder
    
    running 2 tests
    test add_two_0 ... ok
    test _0 ... ok
    
    test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured
    
现在我们已经知道3种测试了，注册这个document test的名字: _0是模块测试的名字，add_two_0是函数测试，如果你添加更多测试他们会自动加1 类似add_two_1



       
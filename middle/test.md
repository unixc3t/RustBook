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
    

    

















       
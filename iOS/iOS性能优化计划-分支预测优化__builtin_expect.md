# iOS性能优化计划-分支预测优化__builtin_expect

将流水线引入cpu，可以提高cpu的效率。更简单的说，让cpu可以预先取出下一条指令，减少cpu等待取指令的耗时，从而可以提供cpu的效率。

如果存在跳转指令，那么预先取出的指令就无用了。cpu在执行当前指令时，从内存中取出了当前指令的下一条指令。执行完当前指令后，cpu发现不是要执行下一条指令,而是执行offset偏移处的指令。cpu只能重新从内存中取出offset偏移处的指令。因此，跳转指令会降低流水线的效率，也就是降低cpu的效率。

综上，在写程序时应该尽量避免跳转语句。那么如何避免跳转语句呢？答案就是使用__builtin_expect。

这个指令是gcc引入的，作用是"允许程序员将最有可能执行的分支告诉编译器"。这个指令的写法为：`__builtin_expect(EXP, N)`。意思是：EXP==N的概率很大。

一般的使用方法是将__builtin_expect指令封装为LIKELY和UNLIKELY宏。这两个宏的写法如下。

```
        #define LIKELY(x) __builtin_expect(!!(x), 1) //x很可能为真
        #define UNLIKELY(x) __builtin_expect(!!(x), 0) //x很可能为假
12
```

综上而言，__builtin_expect是为了生成高效的代码。
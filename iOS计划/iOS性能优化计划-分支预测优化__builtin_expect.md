# iOS性能优化计划-分支预测优化__builtin_expect

最近研究 iOS runtime 源码的时候，发现源码经常使用 `fastpath(x)` 和 `slowpath(x)` 这两个宏来进行判断：

```c++
#define fastpath(x) (__builtin_expect(bool(x), 1))
#define slowpath(x) (__builtin_expect(bool(x), 0))
```

`__builtin_expect()` 是 GCC 的一个内建函数，用于帮助程序员优化分支预测，其声明如下：

```c++
long __builtin_expect(long exp, long c);
```

`__builtin_expect()` 的意思是告诉编译器 `exp == c` 的可能性大，其中 `exp` 是一个整型表达式，`c` 必须是一个编译期常量。

`__builtin_expect()` 通常在 `if-else` 中使用， `if (exp)` 等价于 `if (__builtin_expert(exp, c))`，`c   ` 表示期望值，`c` 为1表示期望大，`c` 为0表示期望小。

cpu 是利用流水线进行指令处理，因此 cpu 可以提前取多条指令进行并行处理，但遇到跳转指令时就需要重新取指令，跳转指令打乱了 cpu 流水线，增加了取指令的耗时，降低了 cpu 的效率。

因此，跳转指令数少的程序拥有更高的执行效率。

我们肯定会不可避免地使用 `if-else` ，而 `if-else` 编译后，一个分支的汇编代码会紧随前面的代码，而另一个分支的汇编代码需要使用跳转指令才能访问。

在复杂的程序中会有很多的 `if-else`，又或是一个有 `if-else` 的库函数，每秒钟被调用几万次，程序员在分支预测方面做得不好，编译器又不能精准的预测每一个分支，这时跳转指令的消耗就会很大，程序的执行效率就不高。而 `__builtin_expect()` 就是帮助优化分支预测。

`__builtin_expect()` 会将**最有可能执行的分支**告诉编译器，编译器在编译时就会把**最有可能执行的分支**的代码紧跟在前面的代码后面，而另一个分支则使用跳转指令访问，从而减少跳转指令带来的性能损耗。

所以我们应该多使用 `__builtin_expect()` 用于条件判断，以此程序执行的效率。

举栗子：表示 x > 0 的可能很大，所以执行 `branch 1` 的代码速度更快。

```c++
if (__builtin_expect(x > 0, 1)) {
    //branch 1
} else {
    //branch 2
}
```

举栗子：表示 x > 0 的可能很小，所以执行 `branch 2` 的代码速度更快。

```c++
if (__builtin_expect(x > 0, 0)) {
    //branch 1
} else {
    //branch 2
}
```
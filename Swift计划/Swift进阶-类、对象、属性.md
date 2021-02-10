# Swift进阶-类、对象、属性

在终端 cd 到 main.swift 文件下，然后通过 `swiftc -emit-sil` 命令将 main.swift 转为 main.sil

```shell
swiftc -emit-sil main.swift >> ./main.sil
```



`@_hasStorage` 存储属性

`@_hasInitialValue` 具有初始值

`sil_global hidden @$s4main6personAA6PersonCvp : $Person` 是 main.swift 中的全局实例对象 person



看着 `s4main6personAA6PersonCvp` 感觉有点长有点怪异，这应该是一种特定的重命名，

~~s4~~**main**~~6~~**person**~~AA6~~**Person**~~Cvp~~ main 文件下 Person 类型的 person 实例对象。

`xcrun swift-demangle s4main6personAA6PersonCvp`

```shell
s4main6personAA6PersonCvp ---> main.person : main.Person
```



```swift
//`@main`：标识当前main.swift的`入口函数`，SIL中的标识符名称以`@`作为前缀
```



%0、%1、%2在 SIL 中表示为寄存器，可以理解为SIL的常量，一旦赋值不可修改，如果想要继续使用，就需要不断的累加数字。

SIL 中的寄存器是虚拟寄存器，只有在真正运行才会使用到 register read 中的真实寄存器。



在 Swift 中，Int、String、Double 都是结构体


# Swift进阶-SIL分析

## Swift编译过程 ##

![image-20201214231614598](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201214231614598.png)

`*.swift` 首先在前端编译器 `swiftc` 的词法、语法、语义分析形成 `AST`（Swift 抽象语法树），然后通过 `SILGen` 生成 `SIL`，`SIL` 通过优化器优化后，再通过 `IRGen` 转成 `IR`，`IR` 最后通过后端编译器 `LLVM` 编译成不同平台的 .o 二进制可执行文件。

在这个过程中，我们发现可以通过 `SIL` 和 `IR` 来研究 Swift 的底层原理。

## SIL 简介

[SIL 语法官方文档](https://github.com/apple/swift/blob/main/docs/SIL.rst#abstract)

SIL (Swift intermediate language) 是 Swift 编译过程中生成的中间代码，主要用于进一步分析和优化 Swift 代码。

我们可以通过 `swiftc -h` 终端命令，查看swiftc的所有命令：

```shell
  -dump-ast              解析文件和类型检查，并打印AST语法树
  -dump-parse            解析文件，并打印AST语法树
  -dump-pcm              转储有关预编译Clang模块的调试信息
  -dump-scope-maps       解析文件和类型检查，并打印 the scope map
  -dump-type-info        从所有导入模块输出固定大小类型的YAML转储
  -dump-type-refinement-contexts 类型检查输入文件，转储类型优化上下文
  -emit-assembly         输出一个 .s 汇编文件
  -emit-bc               输出一个 LLVM BC 文件
  -emit-executable       输出一个 .o 可执行文件
  -emit-imported-modules 输出导入的模块列表
  -emit-ir               输出一个 .ll IR文件
  -emit-library          输出一个 dylib 动态库
  -emit-object           输出一个.o 机器文件
  -emit-pcm              从模块映射输出预编译的Clang模块
  -emit-sibgen           输出序列化的AST语法树和一个原始的 .sil SIL文件
  -emit-sib              输出序列化的AST语法树和一个规范的 .sil SIL文件
  -emit-silgen           输出一个原始的 .sil SIL文件
  -emit-sil              输出一个规范的 .sil SIL文件
  -index-file            为源文件生成索引数据
  -parse                 解析文件
  -print-ast             解析文件和类型检查，并打印简洁的语法树
  -resolve-imports       解析import导入的文件
  -typecheck             语法分析和类型检查，
```

## SIL 简析 ##

新建一个命令行工具项目在 main.swift 中实现以下代码：

```swift
import Foundation

class Person {
    var name: String = "SunSatan"
    var age: Int = 18
}

var person = Person()
```

cd 到 main.swift 目录下，使用 main.swift 代码生成一个规范的 main.sil：

```
swiftc -emit-sil main.swift >> ./main.sil
```

==注：通常使用 VSCode 查看 .sil 文件，安装插件 `VSCode SIL` 可以使 SIL 语法高亮。==


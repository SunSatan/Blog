# Swift 基础计划-关键字和符号 #

## 用在声明中的关键字 ##

- let：用于声明常量。
- var：用于声明变量。
- import：用于声明模块导入。
- struct：用于声明结构体。
- enum：用于声明枚举。
- class：用于声明类，以及。
- static：用于声明。
- protocol：用于声明协议。
- extension：用于声明类、结构体、枚举、协议的拓展。
- func：用于声明函数。
- init：用于声明初始化函数。
- deinit：用于声明反初始化函数（类似 dealloc ）。
- private：用于声明属性、类型、函数等在上一级声明中为私有权限，上一级声明外无法访问。
- fileprivate：用于声明属性、类型、函数等在当前文件中为私有权限，当前文件外无法访问。
- open：用于声明属性、类型、函数等为开放权限，能任意访问。
- public：用于声明属性、类型、函数等为公开权限，能任意访问。
- internal：用于声明属性、类型、函数等为定义模块中的可使用权限，模块之外无法访问。
- inout：用于声明参数为输入输出形式参数，可以在函数内修改参数值并同时作用于函数外。
- associatedtype：关联类型。
- operator：操作符。
- subscript：用于声明类、结构体、枚举的下标函数，可以通过下标对类、结构体、枚举进行读写（类似数组）。
- typealias：用于为已经存在的特定类型重新命名。
- @objc：用于声明类型、方法、属性、协议等在 Objective-C 中调用，但有局限。
- @discardableResult：用于取消函数不使用返回值的警告。

## 用在语句中的关键字 ##

- break：
- continue：
- switch-case：
- case：
- default：
- defer：
- do：
- do-while：
- if-else：
- fallthrough：
- for-in：
- guard：
- in：
- repeat：
- return：
- where：

## 用在表达式和类型中的关键字 ##

- nil
- true
- false
- super
- self
- Self
- is
- as
- try-catch
- dynamicType
- throw
- throws
- rethrows

## 用在模式中的关键字 ##

- _

## 起始于数字标记（ # ）的关键字 ##

- #available
- #column
- #if
- #elseif
- #else
- #endif
- #file
- #function
- #selector
- #line
- #sourceLocation

## 特定上下文中被保留的关键字 ##

- none
- weak
- unowned
- override
- set
- get
- willSet
- didSet
- lazy
- left
- right
- optional
- convenience
- dynamic
- final
- indirect
- mutating
- nonmutating
- postfix
- prefix
- infix
- precedence
- Protocol
- required
- Type
- associativity

## 以下标记被当作保留符号，不能用于自定义操作符 ##

###  ( 、 ) 、 { 、 } 、 [ 、 ] 、 . 、 , 、 : 、 ; 、 = 、 @ 、 # 、 & （作为前缀操作符）、 -> 、 ` 、 ? 和 !（作为后缀操作符）。 ###

在一个没有显式形式参数名的闭包里，参数会隐式地命名为 $0、 ​\$1、\$2 等等。这些参数名在闭包的范围内是有效的。
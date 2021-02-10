# 欢迎来到 Swift 的世界

## Hello, Swift!

```swift
print("Hello, Swift!")
```

## 注释

Swift 中的注释和 C 的注释基本相同。单行注释用两个斜杠开头（ *//* ），多行的注释以（ /* ）开头，以（**/* ）结尾。

Swift 中的多行注释还可以内嵌在其它的多行注释之中。

```swift
/* 这是第一个多行注释的开头
/* 这是第二个嵌套在内的注释块 */
这是第一个注释块的结尾 */
```

内嵌多行注释，可以便捷地注释掉一大段代码块，即使这段代码块中已经有了多行注释。

## 变量和常量

常量和变量必须在使用前被声明，使用 `let` 来声明一个常量，用 `var` 来声明一个变量。

值的类型不用总显式的写出来，在声明常量或变量的时候，直接给它们赋值就可以让编译器推断它们的类型。

```swift
var myVariable = 42
let myConstant = 42
```

当初始值不满足类型要求，或者声明时不赋初始值，就需要在变量名后显式的写出来，并冒号分隔。

```swift
let myConstant: Double = 70
```

当一个变量或者常亮确定了类型以后，就绝对不会隐式地转换为其他类型。如果需要将值转换为不同的类型，需要使用显示地转换声明。

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

如果不使用显示地转换声明，将两个不同类型的值进行操作，那么就会报如下错误：

```swift
Binary operator '+' cannot be applied to operands of type 'String' and 'Int'
```

我们可以在一行中声明多个变量或常量，用逗号分隔：

```swift
var x = 0.0, y = 0.0, z = 0.0
```

我们可以在一行中定义多个相关的变量为相同的类型，用逗号分隔，只要在最后的变量名字后边加上类型标注。

```swift
var red, green, blue: Double
```

我们几乎可以使用任何字符来为常量和变量进行命名，甚至包括 Unicode 字符：

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶️🐮️ = "dogcow"
```

但是，常量和变量的名字不能包含空白字符、数学符号、箭头、保留的（或者无效的）Unicode 码位、连线和制表符，也不能以数字开头。

Swift 并不强制每一句代码结尾都使用（ ; ）分隔，但是想在一行里写多句代码，那么（ ; ）还有必要的。

```swift
let cat = "🐱️"; print(cat)
```

## 基础类型

Swift 为所有 C 和 Objective-C 的类型提供了自己的版本，包括整型值的 `Int`，浮点数值的 `Double` 和 `Float`，布尔量值的`Bool` ，字符串值的 `String`。Swift 同样也为三个主要的集合类型提供了更高效的版本， `Array` ，`Set` 和 `Dictionary`。

### 整型

Swift 提供了8，16，32 和 64 位编码的有符号和无符号的整型，例如：`UInt8`指 8 位无符号整型 ，`Int32`指 32 位有符号整型。因此 Swift 不在提供有符号和无符号的长整型。

绝大多数情况下，不需要在代码中为整型设置一个特定的长度。

Swift 提供了一个额外的整型：`Int/UInt`，它拥有与当前平台的原生字相同的长度。

- 在32位平台上， `Int/UInt`的长度和`Int32/UInt32`相同。
- 在64位平台上， `Int/UInt`的长度和`Int64/UInt64`相同。

除非需操作特定长度的整数，否则请尽量使用 Int/UInt，这样能提高代码的统一性和兼容性，

我们可以通过 min 和 max 属性来访问每个整型的最小值和最大值：

```swift
let minValue = UInt8.min // 最小值是 0,   值的类型是 UInt8
let maxValue = UInt8.max // 最大值是 255, 值得类型是 UInt8
```

这些属性的值都是自适应大小的数字类型，因此可以在表达式中与在其他相同类型值同用。

只在一定要用当前平台原生字节长度相同的无符号整数时才使用`UInt`。

其他情况下，推荐统一使用`Int`，统一使用`Int`会提高代码的兼容性，同时可以避免不同数字类型之间的转换问题，也符合整数的类型推断。

因为我们通常只有在特殊情况下才会使用其他整型，例如需要处理外部长度明确的数据或者为了优化性能、内存占用等其他必要情况。在这些情况下，使用指定长度的类型可以帮助你及时发现意外的值溢出和隐式记录正在使用数据的本质。

### 浮点数

Swift 提供了两种有符号的浮点数类型。

- `Double`代表 64 位的浮点数。
- `Float`代表 32 位的浮点数。

`Double`有至少 15 位数字的精度，而`Float`的精度只有 6 位。推荐统一使用`Double`。

### 布尔值

Swift 提供一个基础的布尔量类型，就是`Bool` 。Swift 为布尔值提供了两个常量值，`true`和`false`。

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

Swift 的类型安全机制会阻止你用一个非布尔量的值替换掉Bool：

```swift
if 1 {
   // Cannot convert value of type 'Int' to expected condition type 'Bool'
}
```

## 数值型字面量

整型字面量可以写作：

- 一个十进制数，没有前缀
- 一个二进制数，前缀是 `0b`
- 一个八进制数，前缀是 `0o`
- 一个十六进制数，前缀是 `0x`

```swift
let decimalInteger = 17
let binaryInteger = 0b10001   // 17 in binary notation
let octalInteger = 0o21       // 17 in octal notation
let hexadecimalInteger = 0x11 // 17 in hexadecimal notation
```

浮点数字面量可以是十进制或者是十六进制。小数点两边必须有至少一个十进制数字或者是十六进制的数字。

十进制的浮点数字面量的指数是可选的，用 e/E 表示；十六进制的浮点数字面量必须有指数，用 p/P 来表示。

十进制数与 exp 的指数，结果就等于基数乘以 10^exp^：

- 1.25e2 == 1.25 x 10^2^ == 125.0 .
- 1.25e-2 == 1.25 x 10^-2^ == 0.0125 .

十六进制数与 exp 指数，结果就等于基数乘以2^exp^：

- 0xFp2 == 15 x 2^2^ == 60.0 .
- 0xFp-2 == 15 x 2^-2^ == 3.75 .

整数和浮点数都可以添加额外的零或者添加下划线来增加代码的可读性：

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## 数值类型转换



### 整数转换

众所周知，不同的整型存储的范围是不同的。 `Int8`可以存储的范围是`-128~127`，而`UInt8`能存储的范围是`0~255`。

如果值超出了可存储的范围，编译的时候就会报错：

```swift
let cannotBeNegative: UInt8 = -1
// Negative integer '-1' overflows when stored into unsigned type 'UInt8'
let tooBig: Int8 = Int8.max + 1
// Arithmetic operation '127 + 1' (on type 'Int8') results in an overflow
```

所以我们必须根据不同的情况，显示地进行数值类型的转换。这种选择性使用的方式可以避免隐式转换的错误，并使我们的代码中的类型转换意图更加清晰，因此类型不同的值不能直接进行运算：

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + one
// Binary operator '+' cannot be applied to operands of type 'UInt16' and 'UInt8'
```

我们要使用`UInt16(one)`创建一个新的`UInt16`类型并用`one`的值初始化，这样就可以在原来的地方使用了：

```swift
let twoThousandAndOne = twoThousand + UInt16(one)
```

`SomeType(ofInitialValue)`是调用 Swift 类型初始化器并传入一个初始值的默认方法。

例如在 Swift 内部， `UInt16`有一个初始化器，可以接受一个`UInt8`类型的值来进行初始化，所以可以用`UInt8`来创建一个新的`UInt16`。

这里需要注意的是并不能传入任意类型的值，只能传入`UInt16`内部有对应初始化器的值。不过你可以扩展现有的类型来让它可以接收其他类型的值（包括自定义类型）。

### 浮点数转换

同样的，`Double`和`Float`相互之间也不能直接使用：

```swift
let twoThousand: Double = 2_000
let one: Float = 1
let twoThousandAndOne = twoThousand + one
// Binary operator '+' cannot be applied to operands of type 'Double' and 'Float'
```

### 整数和浮点数转换

整数和浮点数的转换也必须显式地指定类型：

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
```

众所周知，浮点数在转换为整数时，小数点会被截断。

**注意：数字常量和变量，与数字字面量是不同，数字字面量 3 可以直接和字面量 0.14159 相加，因为数字字面量本身没有明确的类型，而它们的类型只有在编译器需要计算的时候才会被推测出来。**

## 类型别名

类型别名可以为已经存在的类型定义了一个新的可选名字。使用`typealias`定义类型别名。

```swift
typealias AudioSample = UInt16
```

一旦为类型创建了一个别名，你就可以在任何使用原始名字的地方使用这个别名。

## 元组

元组可以把多个值合并成单一的复合型的值。元组内的值可以是任何类型，而且可以不必是同一类型。

```swift
let http404Error = (404, "Not Found")
```

(404, "Not Found")元组把一个`Int`和一个`String`组合起来表示 HTTP 状态代码的两种不同的值：数字和人类可读的描述。它可以被描述为“一个类型为`(Int, String)`的元组”。

任何类型的排列都可以被用来创建一个元组，它可以包含任意多的类型。例如`(Int, Int, Int)` 和 `(String, Bool)`，任何类型的组合都是可以的。

我们可以将一个元组的内容分解成单独的常量或变量：

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
print("The status message is \(statusMessage)")
```

当我们分解元组的时候，如果只需要使用其中的一部分数据，不需要的数据可以用下滑线（ _ ）代替：

```swift
let (justTheStatusCode, _) = http404Error
```

我们还可以利用从零开始的索引访问元组中的单独元素：

```swift
print("The status code is \(http404Error.0)")
print("The status message is \(http404Error.1)")
```

我们也可以在定义元组的时候给其中的单个元素命名，然后通过访问名字来获取元素的值：

```swift
let http200Status = (statusCode: 200, description: "OK")
print("The status code is \(http200Status.statusCode)")
print("The status message is \(http200Status.description)")
```

作为函数返回值时，元组非常有用。相比只能返回一个类型的值，元组能包含两个不同类型值，他可以让函数的返回信息更有用。

**元组在临时的值组合中很有用，但是它们不适合创建复杂的数据结构。**

**如果你的数据结构超出了临时使用的范围，那么请建立一个类或结构体来代替元组。**

## 类型安全和类型推断

Swift 是一门类型安全的语言。类型安全的语言可以让你清楚地知道代码可以处理的值的类型。如果你的代码期望获得`String` ***，***你就不能错误的传给它一个`Int`。

因为 Swift 是类型安全的，他在编译代码的时候会进行类型检查，任何不匹配的类型都会被标记为错误。这会在开发阶段帮助你更早的发现并修复错误。

当你操作不同类型的值时，类型检查能帮助你避免错误。当然，这并不意味着你得为每一个常量或变量声明一个特定的类型。如果你没有为所需要的值进行类型声明，Swift 会使用类型推断的功能推断出合适的类型。通过检查你给变量赋的值，类型推断能够在编译阶段自动的推断出值的类型。

因为有了类型推断，Swift 和 C、Objective-C 相比，我们只需要少量的显示的类型声明，大部分常量和变量的类型声明，Swift 在编译阶段已经帮我们做了。

Swift 在推断整数时始终选择`Int`，在推断浮点值时始终会选择 Double。这也是推荐统一使用`Int`和`Double`的原因。

## 可选项

可以利用可选项来处理值可能缺失的情况。可选项意味着：这里有一个值等于x，或者这里根本没有值等于`nil`。

在 C 和 Objective-C 中，没有可选项的概念。在 Objective-C 中有一个近似的特性，一个方法可以返回一个对象或者`nil`。然而，`nil`只能用在对象上，却不能作用在结构体、基础类型和枚举值上。

而 Swift 中的可选项就可以让你知道任何类型的值的缺失，并且不需要一个特殊的值。

我们可以在类型声明后面添加（ ? ），表示可选项：

```swift
let number: Int? = 404
```

（ ? ）明确了它储存的值是一个可选项，意思就是说它要么包含某个`Int`值，要么根本不包含值，他绝对不会包含其他类型的值，例如`Bool`值或者`String`值。

在类型转换中就可能造成值的缺失，例如 "Hello, Swift!” 无法转换为`Int`，初始化失败后返回`Int?`，而不是`Int`。

```swift
let possibleNumber = "Hello, Swift!"
let convertedNumber = Int(possibleNumber)
```

### nil

Swift 中的`nil`和 Objective-C 中的 `nil` 不同，在 Objective-C 中 `nil` 是一个指向不存在对象的指针。在 Swift 中，`nil`不是指针，是值缺失的一种特殊类型，任何类型的可选项都可以设置成`nil`。

你可以通过给可选项变量赋值`nil`，来设置没有值：

```swift
let number: Int? = nil
```

如果你定义的可选变量没有提供一个默认值，变量会被自动设置成`nil`：

```swift
let number: Int?
```

`nil`只能用于可选项，所以如果变量或常量在某个场景下可能没有值，那么就为它声明为相应类型的可选项。

### 可选项的强制展开

我们一般利用`if`语句通过比较`nil`来判断一个可选中是否包含值，一旦确定可选中包含有值，我们可以在可选项后面加一个感叹号 （ ! ） 来获取值，感叹号的意思是“我知道这个可选项里边有值，展开吧！”，这就是可选项的强制展开。

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
```

### 可选项绑定

可以使用**可选项绑定**来判断可选项是否包含值。

可选绑定可以与`if`和`while`来检查可选项内部的值，并赋值给一个变量或常量：

```swift
if let constantName = someOptional { 
    print("\'\(someOptional)\' has an integer value of \(constantName)")
} 
```

### 隐式展开可选项

有时，在一些程序结构中可选项一旦被设定值之后，就会一直拥有值。在这种情况下，就可以去掉检查的需求，也不必每次访问的时候都进行展开，因为它可以安全的确认每次访问的时候都有一个值。

这种类型的可选项被定义为**隐式展开可选项**。通过在声明的类型后边添加一个叹号（ String! ）而非问号（ String? ） 来书写隐式展开可选项。

```swift
let possibleString: String? = "An optional string." //非隐式可选声明
let forcedString: String = possibleString! // 使用时需要强制展开
 
let assumedString: String! = "An implicitly unwrapped optional string." //隐式展开可选声明
let implicitString: String = assumedString // 直接使用，不需要强制展开
```

隐式展开可选项主要被用在 Swift 类的初始化过程中，可以让可选项被定义的时候就能立即确认其中有值，使用时便简单了。

隐式展开可选项就是在每次访问它的时候被给予了可以自动强制展开的权限：声明可选项时使用了叹号，就不用每次访问可选项时再使用叹号。

当你使用隐式展开可选项值时，Swift 首先尝试将它作为普通可选值来使用；如果它不能作为可选项，Swift 就强制展开值。

在上面的代码中，可选值`assumedString`在给 `implicitString` 赋值前强制展开，因为`implicitString`有显式的非可选String类型。在下面的代码中， `optionalString`没有显式写明类型，所以它是普通可选项。

```swift
let optionalString = assumedString
```

不要在一个变量将来会变为`nil`的情况下使用隐式展开可选项。

## 错误处理

在程序执行阶段，我们可以使用错误处理机制来为错误状况负责。

错误处理机制允许你判断错误的形成原因，在必要的情况下，还能将你的代码中的错误传递到程序的其他地方。

通过在函数声明过程当中加入`throws`关键字来表明这个函数会抛出一个错误。当这个函数遇到错误情况，就会抛出一个错误，函数的访问者会可以捕捉到这个错误，并作出合适的反应。

```swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

当你调用了一个可以抛出错误的函数时，需要在表达式前预置 `try`关键字。Swift 会自动将错误传递到它们的生效范围之外，直到它们被 catch 分句处理。

```swift
do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

`do`语句创建了一个新的容器范围，可以让错误被传递到到不止一个的`catch`分句里。

下面的栗子演示了如何利用错误处理机制处理不同的错误情况：

```swift
func makeASandwich() throws {
    // ...
}
 
do {
    try makeASandwich()
    eatASandwich()
} catch Error.OutOfCleanDishes {
    washDishes()
} catch Error.MissingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

在上面的栗子中，在没有干净的盘子或者缺少原料的情况下，方法`makeASandwich()`就会抛出一个错误。

由于`makeASandwich()`的抛出，方法的调用被包裹在了一个`try`的表达式中。通过将方法的调用包裹在 do 语句中，任何抛出来的错误都会被传递到预先提供的`catch`分句中。

如果没有错误抛出，方法`eatASandwich()`就会被调用；如果抛出`Error.OutOfCleanDishes`，就执行`washDishes()`方法；如果抛出`Error.MissingIngredients`，那么`buyGroceries(_:)` 就会协同被`catch`模式捕获的 [String] 值一起调用。

## 断言和先决条件

**断言**和**先决条件**可以用来保证在执行后续代码前某必要条件是满足的，所以我们可以使用断言和先决条件来验证期望和假定。

如果判断条件在断言或先决条件中为`true`，代码就可以继续正常执行。

如果判断条件为`false` ，那么程序当前的状态就是非法的，代码执行立即结束，app 终止。

断言能够帮助你在开发的过程中找到错误和不正确的假定，先决条件帮助你探测产品的问题。

在运行时帮助你额外验证你的期望，断言和先决条件同样是代码中好用的证明形式。

不同于**错误处理**，断言和先决条件并不用于可回复或者期望的错误。由于错误断言或先决条件显示非法的程序状态，所以没办法来抓取错误断言。

使用断言和先决条件不能代替你代码中小概率非法情况的处理设计。

总之，使用他们来强制数据和状态正确会让你的 app 在有非法状态时终止的更可预料，更好的 debug。

**断言和先决条件的不同之处在于他们什么时候做检查：断言只在 debug 构建的时候检查，但先决条件则在 debug 和生产构建中生效。在生产构建中，断言是无效的。这让我们可以在开发的过程当中随便使用断言，而无需担心影响性能。**

### 使用断言进行调试

顾名思义，断言可以“断言”一个条件是否为真，会在运行的时候检查一个逻辑条件是否为`true`。

我们可以使用断言确保在运行其他代码之前，必要的条件已经被满足。

如果你的代码在调试环境下触发了一个断言，你可以明确的知道不可用的状态发生在什么地方，还能检查断言被触发时你的应用的状态。另外，断言还可以附加一条断言调试的信息。

我们可以使用全局函数`assert(_:_:`)来写断言，向`assert(_:_:)`函数传入一个逻辑表达式和断言为`false`时的提示信息：

```swift
let age = -3
assert(age >= 0, "A person's age cannot be less than zero")
```

### 强制先决条件

在代码中任何条件可能潜在为假但必须*肯定*为真才能继续执行的地方使用先决条件。例如可以使用先决条件来检测下标有没有越界，或者检测函数是否收到了一个合法的值。

我们可以使用全局函数`precondition(_:_:)`来写先决条件，向`precondition(_:_:)`函数传入一个逻辑表达式和断言为`false`时的提示信息：

```swift
precondition(index > 0, "Index must be greater than zero.")
```

我们还可以调用`preconditionFailure(_:)`函数来标明错误发生了。

如果你在不检查模式编译（ -Ounchecked ），先决条件就不会检查。编译器假定先决条件永远为真，并且它根据你的代码进行优化。总之，`fatalError(_:file:line:)`函数一定会终止执行，无论你优化设定如何。

你可以在早期开发过程中使用`fatalError(_:file:line:)`函数来标记那些还没实现的功能。由于致命错误永远不会被优化，不同于断言和先决条件，你可以确定执行遇到这些临时占位永远会停止。

## 基本运算符

### 赋值运算符

单个元素的赋值自不必多说。

如果赋值符号右侧是拥有多个值的元组，它的元素将会一次性地拆分成常量或者变量进行赋值：

```swift
let (x, y) = (1, 2) // x 等于 1, 同时 y 等于 2
```

与 Objective-C 和 C 不同，Swift 的赋值符号自身不会返回值。下面的语句是不合法的：

```swift
if x = y {
    // 这是不合法的, 因为 x = y 并不会返回任何值。
}
```

这个特性避免了赋值符号 ( ***`=`*** ) 被当成等于符号 ( ***`==`*** ) 使用。

### 算术运算符

Swift 对所有的数字类型都支持四种标准算术运算符：加 ( + )、减 ( - )、乘 ( * )、除 ( / )

与 C 和 Objective-C 中的算术运算符不同，Swift 算术运算符默认不允许值溢出。

你也可以选择使用 Swift 的溢出操作符（比如 `a &+ b`  ）来行使溢出行为。

加法运算符同时也支持 String 的拼接：`"hello, " + "world"` 。

### 余数运算符

*余数运算符*（ a % b ）可以求出多少个 b 的倍数能够刚好放进 a 中并且返回剩下的值（就是我们所谓的余数）。

现在我们展示余数运算符如何生效：要计算 9 % 4，你首先要求出多少个 4 能够放到 9 里边：

你可以给 9 当中放进两个 4 去，这样就得到了 1 这个余数 （橘黄色的部分）。

在 Swift 中，这将写作：`9 % 4` 。

决定 a % b` `的结果， % 按照如下等式运算然后返回 remainder 作为它的输出：

$a = (b * multiplier) + remainder$

其中，multiplier 是 b 能放进 a 的最大倍数，把 a=9 和 b=4 插入到等式当中去：9 = (4 x 2) + 1 。

当 a 是负数时也使用相同的方法来进行计算：-9 = (4 x -2) + -1，得到余数 -1 。

当 b为负数时它的正负号被忽略掉了。这意味着 a % b 与 a % -b 能够获得相同的答案。

### 一元减号运算符

数字值的正负号可以用前缀（ - ）来切换，不加任何空格。

### 一元加号运算符

一元加号运算符（ + ）作为返回它操作的值，不会对其进行任何的修改：

尽管一元加号运算符实际上什么也不做，你还是可以对正数使用它来让你的代码对一元减号运算符来说显得更加对称。

### 组合赋值符号

如同 C ，Swift 提供了由赋值符号（ = ）和其他符号组成的组合赋值符号：（ += ）、（ -= ）、（ *= ）、（ /= ）、（ %= ）。

### 比较运算符

Swift 支持所有 C 的标准比较运算符：相等 ( a == b )、不相等 ( a != b )、大于 ( a > b )、小于 ( a < b )、大于等于 ( a >= b )、小于等于 ( a <= b )。

Swift 还提供两个等价运算符（ === 和 !\== ），可以使用它们来判断两个对象的引用是否相同。

你可以比较拥有同样数量值的元组，只要元组中的每个值都是可比较的。

比如说， Int 和 String 都可以用来比较大小，也就是说 (Int,String) 类型的元组就可以比较。

一般来说， Bool 不能比较，这意味着包含布尔值的元组不能用来比较大小。

元组以从左到右的顺序比较大小，一次一个值，直到找到两个不相等的值为止。如果所有的值都是相等的，那么就认为元组本身是相等的。

Swift 标准库包含的元组比较运算符仅支持小于七个元素的元组。

要比较拥有七个或者更多元素的元组，你必须自己实现比较运算符。

### 三元条件运算符

三元条件运算符是一种有三部分的特殊运算，它看起来是这样的：

```swift
 question ? answer1 : answer2 
```

这是一种基于 question 是真还是假来选择两个表达式之一的便捷写法。

如果 question 是真，则以 answer1 为返回值；否则以 answer2 为返回值。

三元条件运算符就是下边代码的简写：

```swift
if question {
    answer1
} else {
    answer2
}
```

三元条件运算符提供了一个非常有效的简写来决策要两个表达式之间选哪个。

但是，使用三元条件运算符要小心。它的简洁性会导致你代码重用的时候失去易读的特性，所以要避免把多个三元条件运算符组合到一句代码当中。

### 合并空值运算符

合并空值运算符 （ a ?? b ）如果可选项 a 有值则展开，如果没有值是 nil ，则返回默认值 b 。表达式 a 必须是一个可选类型。表达式 b 必须与 a 的储存类型相同。

合并空值运算符是下边代码的缩写：

```swift
a != nil ? a! : b
```

上边的代码中，使用三元条件运算符强制展开（ a! ）储存在 a 中的值，如果 a 不是 nil 的话，否则就返回 b 的值。如果 a 的值是非空的， b 的值将不会被考虑。

合并空值运算符提供了更加优雅的方式来封装这个条件选择和展开操作，让它更加简洁易读。

下边的栗子使用了合并空值运算符来在默认颜色名和可选的用户定义颜色名之间做选择：

```swift
let defaultColorName = "red"
var userDefinedColorName: String? // defaults to nil
var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
```

### 区间运算符

Swift 包含了两个区间运算符 ，他们是表示一个范围的值的便捷方式。

#### 闭区间运算符

闭区间运算符（ a...b ）定义了从 a 到 b 的一组范围，并且包含 a 和 b ，其中，a 的值不能大于 b 。

在遍历你需要用到的所有数字时，使用闭区间运算符是个不错的选择，比如说在 for-in 循环当中：

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
```

#### 左闭右开区间运算符

左闭右开区间运算符（ a..<b ）定义了从 a 到 b 但不包括 b 的区间，它只包含起始值但并不包含结束值。

如同闭区间运算符， a 的值也不能大于 b ，同时如果 a 与 b 的值相等，那返回的区间将会是空的。

左闭右开区间运算符在遍历基于零开始序列比如说数组的时候非常有用，它从零开始遍历到数组长度（但是不包含）：

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for i in 0..<names.count {
    print("Person \(i + 1) is called \(names[i])")
}
```

#### 单侧区间

单侧区间是指让区间朝一个方向尽可能的延伸，在这种情况下，你可以省略区间运算符一侧的值。

因为运算符只有一侧有值，比如说：

```swift
for name in names[2...] {
    print(name)
}// Brian Jack

for name in names[...2] {
    print(name)
}// Anna Alex Brian
```

左闭右开区间运算符同样有单侧形式，只需要写它最终的值。

```swift
for name in names[..<2] {
    print(name)
}// Anna Alex
```

单侧区间可以在其他上下文中使用，不仅仅是下标。

但你不能遍历省略了第一个值的单侧区间，因为遍历根本不知道该从哪里开始。

你可以遍历省略了最终值的单侧区间；总之，由于区间无限连续，你要确保给循环添加一个显式的条件。

```swift
let range = ...5    //(-∞, 5]
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```

### 逻辑运算符

Swift 支持三种基于 C 的语言的标准逻辑运算符：逻辑非  ( !a )、逻辑与  ( a && b )、逻辑或  ( a || b )。

## 字符串和字符

### 字符串字面量

字符串字面量是被双引号（ " ）包裹的固定顺序文本字符。

```swift
let someString = "Some string literal value"
```

#### 多行字符串字面量

多行字符串字面量是用三个双引号引起来的一系列字符：

```swift
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.
 
"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```

多行字符串字面量把所有行包括在引号内，也就是说多行字符串的开始或者结束都不会有换行符：

```swift
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""

// 输出都是单行的 These are the same.
```

但是要让多行字符串字面量的起始或结束于换行，就在第一或最后一行多写一个空行：

```swift
let multilineString = """

These are the same.

"""
```

多行字符串可以根据缩进来匹配周围的代码。结束的（ """ ）前的空格会告诉 Swift 其他行前应该有多少空白是需要忽略的，如果某行的空格超过了结束的（ """ ），那么这些空格就会被包含进字符串。

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201011150022399.png" alt="image-20201011150022399" style="zoom:50%;" />

#### 字符串字面量里的特殊字符

- 转义特殊字符 \0 (空字符)， \\\\ (反斜杠)， \t (水平制表符)， \n (换行符)， \r(回车符)， \" (双引号) 以及 \' (单引号)；
- 任意的 Unicode 标量，写作 \u{n}，里边的 n是一个 1-8 个与合法 Unicode 码位相等的16进制数字。

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}" // $, Unicode scalar U+0024
let blackHeart = "\u{2665}" // ♥, Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖️, Unicode scalar U+1F496
```

由于多行字符串字面量使用三个双引号而不是一个作为标记，所以多行字符串字面量中可以直接使用双引号（ “ ）而不需转义。

但要在多行字符串中包含文本（"""） ，则至少需要一个转义：

```swift
let threeDoubleQuotationMarks = """
Escaping the first quotation mark \"""
Escaping all three quotation marks \"\"\"
"""

/* 打印如下：
Escaping the first quotation mark """
Escaping all three quotation marks """
*/
```

### 初始化一个空字符串

为了绑定一个更长的字符串，要在一开始创建一个空的 String 值，要么赋值一个空的字符串字面量给变量，要么使用初始化器语法来初始化一个新的 String 实例：

```swift
var emptyString = ""
var anotherEmptyString = String()
```

通过检查布尔量 `isEmpty` 属性来确认一个 String 值是否为空：

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
```

### 字符串是值类型

Swift 的 String 是一种值类型。如果你创建了一个新的 String 值， String 值在传递给方法或者函数的时候会被复制过去，还有赋值给常量或者变量的时候也是一样。

每一次赋值和传递，现存的 String 值都会被复制一次，传递走的是拷贝的副本而不是原本。

Swift 的默认拷贝 String 的行为保证了当一个方法或者函数传给你一个 String 值时，你就绝对拥有了这个 String值，无需关心它从哪里来到哪里去，你可以确定你传走的这个字符串除了你自己就不会有别人改变它。

另一方面，Swift 编译器优化了字符串使用的资源，实际上拷贝只会在确实需要的时候才进行。这意味着当你把字符串当做值类型来操作的时候总是能够有用很棒的性能。

### 操作字符

你可以通过 for-in循环遍历 String 中的每一个独立的 Character值：

```swift
for character in "Dog!🐶️" {
    print(character)
}
// D
// o
// g
// !
// 🐶️
```

你可以通过提供 Character类型标注来从单个字符的字符串字面量创建一个独立的 Character常量或者变量：

```swift
 let exclamationMark: Character = "!"
```

String 可以直接使用 Character 的数组来初始化：

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱️"]
let catString = String(catCharacters) // "Cat!🐱️"
```

### 连接字符串和字符

String 可以使用（+）来进行拼接：

```swift
var welcome = "hello" + " there" // hello there
```

可以使用 String 的 `append()` 方法来追加 Character 值：

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark) // "hello there!"
```

### 字符串插值

字符串插值是一种从混合**常量**、**变量**、**字面量**和**表达式**的字符串字面量构造新 String值的方法。每一个你插入到字符串字面量的元素都要被一对圆括号包裹，然后使用反斜杠前缀：

```swift
let message = "\(3) times 2.5 is \(Double(3) * 2.5)" // 3 times 2.5 is 7.5
```

注意：你作为插值写在圆括号中的表达式不能包含非转义的反斜杠（\\），并且不能包含回车或者换行符。

### 扩展字符串分隔符

可以在字符串字面量的前后放置扩展分隔符（#），让字符串包含特殊字符而不让它们生效。

```swift
let towLine = #"Line 1\nLine 2"#
// 打印：Line 1\nLine 2
// 而不会打印两行
```

但如果你需要字符串中某个特殊符号的效果，需要在（\）的后面（n）的前面放与字符串字面量的前后数量相等的（#）。

```swift
let towLine = #"Line 1\#nLine 2"#
let towLine = ##"Line1\##nLine2"##
let towLine = ###"Line1\###nLine2"###
```

扩展分隔符（#）同样可以使用多行字符串字面量。你可以在多行字符串字面量中使用扩展分隔符来包含文本""" ，从而避免让它结束字面量。

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

扩展分隔符（#）同样可以作用在字符串插值中：

```swift
print(#"Write an interpolated string in Swift using \(multiplier)."#)
// Prints "Write an interpolated string in Swift using \(multiplier)."

print(#"6 times 7 is \#(6 * 7)."#)
// Prints "6 times 7 is 42."
```

### Unicode

Unicode 是一种在不同书写系统中编码、表示和处理文本的国际标准。它允许你表示几乎标准化格式的任何语言中的任何字符，并且为外部源比如文本文档或者网页读写这些字符。如同这节中描述的那样，Swift 的 String和 Character类型是完全 Unicode 兼容的。

### 字符统计

要在字符串中取出 Character 值的总数，使用字符串的 `count` 属性。

通过 `count` 属性返回的字符统计并不会总是与包含相同字符的 NSString 中 length 属性相同。

### 访问和修改字符串

你可以通过下标脚本语法或者它自身的属性和方法来访问和修改字符串。

#### 字符串索引

每一个 String 值都有相关的索引类型，`String.Index` 相当于每个 Character 在字符串中的位置。

不同的字符会获得不同的内存空间来储存，所以为了明确哪个 Character 在哪个特定的位置，必须使用 `String.Index` 来获取。因此，Swift 的字符串不能通过整数值索引，例如 towLine[1] 会报错。

我们使用 `startIndex` 属性来访问 String 中第一个 Character 的位置。

`endIndex ` 属性就是 String 中最后一个字符后的位置，所以 `endIndex` 属性是越界的。

如果 String 为空，则 `startIndex` 与 endIndex 相等。

使用 index(before:) 和 index(after:) 方法来访问给定索引的前后。

要访问给定索引更远的索引，可以使用index(_:offsetBy:) 方法。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

使用 `indices` 属性来访问字符串中每个字符的索引。

```swift
for index in greeting.indices {
    print("\(greeting[index]) ")
}
// G u t e n   T a g ! 
```

### 子字符串

当你使用方法会获得了一个字符串的子字符串 Substring 的实例，这不是另外一个字符串，因为Substring 和 String 共用一块内存区域，所以需要把提取出来的 Substring  转换为 String 实例。

```swift
let greeting = "Hello, world!"
let index = greeting.index(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index] // Hello

let newString = String(beginning)
```

<img src="/Users/sunsatan/Library/Application%20Support/typora-user-images/image-20201011184808631.png" alt="image-20201011184808631" style="zoom: 33%;" />

#### 字符串比较

Swift 提供了三种方法来比较文本值：字符串和字符相等性，前缀相等性以及后缀相等性。

#### 字符串和字符相等性

字符串和字符相等使用“等于”运算符（ == ）和“不等”运算符（ != ）进行检查：

两个 String值（或者两个 Character值）如果它们的扩展字形集群是*规范化相等*，则被认为是相等的。

如果扩展字形集群拥有相同的语言意义和外形，我们就说它规范化相等，就算它们实际上是由不同的 Unicode 标量组合而成。

#### 前缀和后缀相等性

要检查一个字符串是否拥有特定的字符串前缀或者后缀，调用字符串的 `hasPrefix()` 和 `hasSuffix()` 方法，它们两个都会接受一个 String 类型的实际参数并且返回一个布尔量值。

`hasPrefix()` 和 `hasSuffix()` 方法只对字符串当中的每一个扩展字形集群之间进行了一个逐字符的规范化相等比较。

## 集合类型

Swift 提供了三种主要的集合类型，数组、合集还有字典。数组是有序的值的集合。合集是唯一值的无序集合。字典是无序的键值对集合。

Swift 中的数组、合集和字典总是明确能储存的值的类型以及它们能储存的键。就是说你不会意外地插入一个错误类型的值到集合中去。它同样意味着你可以从集合当中取回确定类型的值。

Swift 的数组、合集和字典是以泛型集合实现的。

### 集合的可变性

`let` 声明的集合就是不可变的，`var` 声明的集合就是可变的，就可以通过添加、移除、替换来改变集合中的元素。

在集合不需要改变的情况下创建不可变集合是个不错的选择。这样做可以允许 Swift 编译器优化你创建的集合的性能。

### 数组

数组以有序的方式来储存相同类型的值。相同类型的值可以在数组的不同地方多次出现。

Swift 数组的类型完整写法是 `Array<Element>`， 简写为 `[Element]`，Element 是数组允许存入的值的类型。

#### 创建一个空数组

你可以使用确定类型通过初始化器语法来创建一个空数组：

```swift
var someInts = [Int]()
```

#### 使用数组字面量创建数组

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
```

#### 使用默认值创建数组

Swift 的 Array 类型提供了初始化器来创建确定大小且元素都设定为相同默认值的数组。

你可以传给初始化器对应类型的默认值（repeating）和新数组元素的数量（count）：

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

你可以通过把两个兼容类型的现存数组用加运算符（ +）加在一起来创建一个新数组。

#### 访问和修改数组

count 只读属性可以得出数组中元素的数量。

isEmpty 布尔量属性可以快捷检查 count属性是否等于 0。

可以通过 append(_:) 方法或 ( += ) 给数组末尾添加新的元素。

可以通过下标从数组中取值、修改值，但不能用下标来追加一个新元素到数组的末尾。

同样可以使用下标来一次改变一个范围的全部值，就算替换与范围长度不同的值的合集也行。

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
```

当数组中元素被移除，任何留下的空白都会被封闭。需要测试。

#### 数组遍历

可以用 `for-in` 循环来遍历整个数组中值的合集：

```swift
for item in shoppingList {
    print(item)
}
```

如果需要值和索引，可以使用元组获取数组的 enumerated() 方法返回值进行遍历。

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
```

### 合集

合集（Set）将同一类型且不重复的值无序地储存一起。

当顺序不重要或者确保元素不会重复的时候，合集（Set）可以代替数组。

#### 合集的哈希值

为了能让类型储存在合集（Set）当中，类型必须是可哈希的，也就是说类型必须提供计算它自身哈希值的方法。

哈希值是 Int 且所有的对比起来相等的对象都一定相同，比如 a == b 必然遵循 a.hashValue == b.hashValue。

所有 Swift 的基础类型（比如 String, Int, Double, 和 Bool）默认都是可哈希的，并且可以用于合集或者字典的键。

自定义的类型只要让遵循 Hashable 协议，即可用于合集或者字典的键。

遵循 Hashable 协议的类型必须提供可获取的叫做 hashValue的 Int 属性。

又因为 Hashable 协议遵循 Equatable 协议，遵循的类型必须同时实现“等于”运算符 ( == ) 。

Equatable 协议需要任何遵循 ( == ) 的实现都具有等价关系。就是说 ( == ) 的实现必须满足以下三个条件，其中 a, b, 和 c是任意值：

- a == a (自反性)
- a == b 意味着 b == a (对称性)
- a == b && b == c 意味着 a == c (传递性)

#### 合集语法

Swift 的合集类型写做 Set<Element>，Element 是合集要储存的类型。不同与数组，合集没有等价的简写。

#### 使用数组字面量创建合集

可以使用数组字面量来初始化一个合集，算是一种写一个或者多个合集值的快捷方式。

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
```

合集类型不能从数组字面量推断出来，所以 Set 必须被显式地声明，而 String 可以被 Swift 类型推断出来，所以初始化可以更简短：

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

#### 访问和修改合集

count、isEmpty、insert(:)、remove(:)、removeAll() 等方法就无需多提。

因为合集是无序的，所以不能使用下标进行访问、修改、删除等。

#### 合集遍历

同数组一样，可以使用 for-in 循环里遍历合集的值。

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
```

如果要以特定的顺序遍历合集的值，需要使用 sorted() 方法把合集元素作为使用 < 运算符排序好的数组返回，然后以数组方式进行遍历。

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
```

#### 合集操作

使用 `intersection(_:)` 方法来创建一个只包含两个合集共有值的新合集。

使用 `symmetricDifference(_:)` 方法来创建一个只包含两个合集各自有的非共有值的新合集。

使用 `union(_:)` 方法来创建一个包含两个合集所有值的新合集。

使用 `subtracting(_:)` 方法来创建一个两个合集当中不包含某个合集值的新合集。

图片

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]
 
oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

#### 合集成员关系和相等性

合集 a 是合集 b 的**超集**，合集 b 是合集 a 的**子集**，因为合集 a 包含合集 b 的所有元素。

合集 b 和合集 c 是不相交的，因为他们的元素没有相同的。

图片

使用“相等”运算符 ( == )来判断两个合集是否包含有相同的值。

使用 isSubset(of:) 方法来确定一个合集的所有值是被某合集包含。

使用 isSuperset(of:)方法来确定一个合集是否包含某个合集的所有值。

使用 isStrictSubset(of:) 或者 isStrictSuperset(of:)方法来确定是个合集是否为某一个合集的子集或者超集，但并不相等。

使用 isDisjoint(with:)方法来判断两个合集是否拥有完全不同的值。

### 字典

字典是无序的互相关联的同一类型的键和同一类型的值的集合。每一个值都与唯一的键相关联，键是值的身份标记。

#### 字典语法

Swift 的字典类型全写为 Dictionary<Key, Value>，简写为 [Key: Value]，其中的 Key 是用来作为字典键的值类型， Value 就是字典为这些键储存的值的类型，Key 必须遵循 Hashable协议。

```swift
var namesOfIntegers = [Int: String]()
```

#### 创建字典

键值对由一个键和一个值组合而成，每个键值对里的键和值用冒号分隔。

键值对写做一个列表，用逗号分隔，并且最终用方括号把它们括起：

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

#### 访问和修改字典

可以通过键来访问、修改、删除字典中的值。

#### 遍历字典

使用 for-in 来遍历字典的键值对时，每一个元素返回为 (key, value) 的元组。

```swift
 for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
```

字典是无序的，要以特定的顺序遍历字典的键或值，就使用键或值的 sorted()方法。

## 控制流

Swift 提供所有多样化的控制流语句，包括 `while`、`for-in `来多次执行任务；`if-else`、`guard`、`switch-case`、`break` 和 `continue` 控制代码分支和切换。

### For-in 循环

使用 for-in 循环来遍历数组、字典、数字区间等。

```
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}

let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}

for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
```

可以使用 stride(from:to:by:) 方法来跳过不想要的区间。

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```

### While 循环

Swift 提供了两种 while 循环：

- while 在每次循环开始的时候判断条件。
- repeat-while 在每次循环结束的时候判断条件。

#### While

```swift
while condition {
    statements
}
```

#### Repeat-While

Swift 的 repeat-while 循环是与其他语言中的 do-while 循环类似的。

```swift
repeat {
    statements
} while condition
```

### if-else

if-else 语句有着一个单一的 if 条件。在 true 时会执行 if 里的语句，false 时会执行 else 里的语句。

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
```

### Switch

switch 语句会将一个值与多个可能的模式匹配。然后基于第一个成功匹配的模式来执行合适的代码块。switch 语句代替 if 语句提供了对多个潜在状态的响应。

switch 语句把一个值与一个或多个相同类型的值比较，每一个 switch 语句都由多个可能的情况组成，每一个情况都以 case 关键字开始。

```swift
switch some value to consider {
case value 1:
    respond to value 1
case value 2, value 3:
    respond to value 2 or 3
default:
    otherwise, do something else
}
```

对于对比额外特定的值来说，Swift 提供了多种方法给每个情况来区别更复杂的匹配模式。

相比 C 和 Objective-C 里的 switch 语句来说，Swift 里的 switch 语句不会默认从每个情况的末尾贯穿到下一个情况里。

相反，整个 switch 语句会在匹配到第一个 switch 情况执行完毕之后退出，不再需要显式的 break 语句。

这使得 switch 语句比 C 的更安全和易用，并且避免了意外地执行多个 switch 情况。

尽管 break 在 Swift 里不是必须的，你仍然可以使用 break 语句来匹配和忽略特定的情况，或者在某个情况执行完成之前就打断它。

每一个情况的函数体必须包含至少一个可执行的语句，不然编译会报错。

在一个 switch 情况中匹配多个值可以用逗号分隔，并且可以写成多行。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
```

## 闭包 ##

**闭包**是可以在你的代码中被传递和引用的功能性独立代码块。

Swift 中的闭包和 C  以及 Objective-C 中的 blocks 很像，还有其他语言中的匿名函数也类似。

闭包能够捕获和存储定义在其上下文中的任何常量和变量的引用，这也就是所谓的*闭合*并包裹那些常量和变量，因此被称为“闭包”，Swift 能够为你处理所有关于捕获的内存管理的操作。

闭包符合如下三种形式中的一种：

- 全局函数是一个有名字但不会捕获任何值的闭包；
- 内嵌函数是一个有名字且能从其上层函数捕获值的闭包；
- 闭包表达式是一个轻量级语法所写的可以捕获其上下文中常量或变量值的没有名字的闭包。

Swift 的闭包表达式拥有简洁的风格，鼓励在常见场景中实现简洁，无累赘的语法。常见的优化包括：

- 利用上下文推断形式参数和返回值的类型；
- 单表达式的闭包可以隐式返回；
- 简写实际参数名；
- 尾随闭包语法

```swift
{ (parameters) -> (return type) in
    statements
}
```


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

### 多行字符串字面量

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
```


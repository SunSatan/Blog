# Swift进阶：Swift源码编译

## 􏰇􏰈􏲥􏲦编译环境

macOS Catalina 10.15.6。

Xcode 12.2（对应 swift-5.3.1-release）。􏰇􏰈􏲥􏲦􏱗􏱘􏱙

VScode。

Homebrew （用于安装 cmake 和 ninja）。

brew install cmake ninja。

## 编译步骤

**第一步：准备工作**

新建一个名为 `swift-source` 的文件夹，**整个路径避免出现中文**。

编译完成后的 Swift 源码大概有50G，请确保硬盘可用空间。

编译过程中，因为要访问外网，所以需要科学上网（各位童鞋自行解决）。

**第二步：clone Swift**

Swift 源码版本需要与 Xcode 版本相匹配，官方编译文档有说明。

因为我的􏰇􏰈􏲥􏲦􏱗􏱘􏱙􏱚􏱛 􏱗􏱘􏱙􏱚Xcode 版本是 12.2，所以我的􏰇􏰈􏲥􏲦􏱗􏱘􏱙􏱚􏱛编译的是 `swift-5.3.1-Release`。

```shell
git clone --branch swift-5.3.1-RELEASE https://github.com/apple/swift.git
```

**第三步：update-checkout**

􏰩􏱐􏰅􏲔􏲕􏲖􏰷这一步非常重要，会 clone 和编译 Swift 的相关库，这一步不成功后面编译 Swift 源码一定会失败。

cd 到 `swift-source` 目录下，􏲓􏰟􏱃􏰕然后终端执行：

```shell
./swift/utils/update-checkout --tag swift-5.3.1-RELEASE --clone
```

**第四步：ninja 编译**

cd 到 `swift-source` 目录下，􏲓􏰟􏱃􏰕然后终端执行：

```shell
./swift/utils/build-script -r --debug-swift-stdlib --lldb
```

该编译过程时间很长，至少三到五个小时，合理安排时间。

<!--因为编译过程可能出现各种问题，所以提供一个已经编译完成的swift-5.3.1-Release。-->

<!--压缩文件11G，解压完成后是45G。确保你的环境和我的一样。-->

```
链接: https://pan.baidu.com/s/1baseJGoj5-bAYPQCgro48A  
密码: vc00
```

**第五步：使用 VSCode 调试 Swift**

首先需要安装 VSCode 的一个插件 CodeLLDB，安装过程需要科学上网。

![image-20201213161117938](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213161117938.png)

使用 VSCode 打开Swift源码文件目录 `swift-source`，然后配置 launch.json。

![image-20201213161453360](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213161453360.png)

确保 launch.json 与下面的 json 一致，只要不乱东，默认就是一致。

```json
{
    "version": "0.2.0",
    "configurations": [
    
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug",
            "program": "${workspaceFolder}/build/Ninja-RelWithDebInfoAssert+stdlib-DebugAssert/swift-macosx-x86_64/bin/swift",
            "args": [],
            "cwd": "${workspaceFolder}"
        }
   ]
}
```

接着开始调试。

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213161808843.png" alt="image-20201213161808843" style="zoom:50%;" />

第一次编译时，VSCode 会安装 CodeLLDB，需要科学上网，右下角有进度条。

直到出现以下的断点和调试信息时，就表示编译成功了。

![image-20201213162018925](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213162018925.png)

过掉断点，打开终端就可以输入swift 代码了，最好从 swift 文件拷贝过来。

![image-20201213162411663](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213162411663.png)

举个栗子：

我们先打开 HeapObject.cpp 中的 `_swift_allocObject_` 方法，并打上断点。![image-20201213163131081](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201213163131081.png)

然后在终端输入我们准备好的代码。

```swift
class Person{
     var age: Int = 18
}
var person = Person()
```


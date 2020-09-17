# iOS底层原理计划-深入理解对象

在 Objective-C 中的对象，主要分为三种基础对象：

1. instance（实例对象）
2. class（类对象）
3. meta-class（元类对象）

## instance（实例对象）

我们从源码看到基类实例对象的结构体定义： 

```c++
/// Represents an instance of a class.
struct objc_object {
    Class _Nonnull isa; //只包含了一个 isa 指针
};
```

`objc_object` 的 `isa` 是指向 `objc_object` 的类对象，原因是：

```objective-c
Class object_getClass(id obj) {
    if (obj) return obj->getIsa();
    else return Nil;
}
```

获取一个 `objc_object` 的类就是获取它的 `isa` 。

### id 和 NSObject

我们常用的 `id` 就是一个 `objc_object` 的指针：

```c++
/// A pointer to an instance of a class.
typedef struct objc_object *id;
```

再看一下 `NSObject` 源码的定义：

```objective-c
@interface NSObject <NSObject> {
    Class isa; //也只包含了一个 isa 指针
}
```

发现其实 `NSObject` 和 `objc_object` 的定义是一样的，是一个结构体在 Objective-C 和 C 中的不同表达。

这也是我们常说 `id` 本质是 `NSObject` 的原因。

### 自定义类的实例对象

我定义了一个 Person 类：

```objective-c
@interface Person : NSObject

@property (nonatomic, assign) char a;   
@property (nonatomic, assign) double b; 
@property (nonatomic, assign) short d;  
@property (nonatomic, assign) int c;    
@property (nonatomic, copy) NSString *f;

@end
```

我们将 .m 文件编译成 cpp 文件，看一下我们自定义的类的结构体是什么：

```c++
struct Person_IMPL {
	struct NSObject_IMPL NSObject_IVARS;
	char _a;
	short _d;
	int _c;
	double _b;
	NSString * _Nonnull _f;
};

struct NSObject_IMPL {
	Class isa;
};
```

通过控制台的变量窗口，我们也可以看出来：

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20200812042916997.png" alt="image-20200812042916997" style="zoom: 67%;" />

其实就是把所有的成员变量加到了结构体中：

```c++
struct Person_IMPL {
	Class isa;
  type _ivar1;
	type _ivar2;
	...
};
```

有没有细心的同学发现结构体中成员变量的顺序和类中属性顺序不一致，这涉及 iOS 对于内存字节对齐的优化。

## class（类对象）

`isa` 的类型`Class` 是一个 `objc_class` 结构体的指针：

```c++
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class;
```

我们从源码可以看到 `objc_class` 的定义： 

```c++
struct objc_class {
    Class isa;              //元类 
		Class superclass;       //父类
    cache_t cache;          //用于缓存指针和 vtable，加速方法的调用
    class_data_bits_t bits; // 存储类的方法、属性、遵循的协议等信息的地方；相当于 class_rw_t 指针加上 rr/alloc 的标志 
}
```

在面向对象中，万物皆是对象。在 Objective-C 中的一个类本质也是一个对象，所以类也可以调用方法， `objc_class` 中也有 `isa` ，它指向类的元类。

`objc_class` 中 `superclass` 指向父类的 `isa`，如果类是根类，那么 `superclass` 为 `NULL` 。

`methodLists` 中存储实例方法列表（实例对象可以调用的方法列表）。

## meta-class（元类对象）

元类对象就是类对象的类，所以叫元类。

元类和类的结构体都是 `objc_class` ，但其中结构体的成员变量的作用有不一样。

元类的 `isa` 指向基类的元类，基类的元类作为所有元类的类，**因此基类的元类也指向自己**。

元类的 `superclass` 指向父类的元类，基类比较特殊：**基类的元类的父类指向基类**。

元类的 `methodLists` 存储类对象的类方法。

元类的 `ivars` 是不使用的，所以类方法中无法访问任何成员变量。

元类的 `instance_size` 默认是 40，不知道有没有什么意义。

## 三种对象的关系

任一以 `objc_class` 指针为开始的结构体，都可以视为一个 objc_object ，一个对象。

下图展示了实例、类、元类的指向和继承关系：

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/20161201152529021.png" alt="11" style="zoom:42%;" />

**实战面试题：下面代码由什么组成？运行结果是什么？**

```objective-c
@interface NSObject(Test)
+ (void)foo;
@end

@implementation NSObject(Test)
- (void)foo {
    NSLog(@"%@", NSStringFromSelector(_cmd));
}
@end
  
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        [NSObject foo];
        [[NSObject new] foo];
    }
    return 0;
}
```

答案：由一个 `NSObject` 的分类 和 主函数 `main` 组成。运行结果是输出两个 `foo`。

解析：`[NSObject foo]` 方法调用过程：首先去 `NSObject` 的元类的方法列表找但没找到，然后根据 **`NSObject` 的元类的`superclass` 是 `NSObject` 的类对象**，所以接着去 `NSObject` 的类对象中找到了 `- (void)foo` 方法调用输出 `foo`。

如果这个分类是写在任意一个非基类的子类里，都会报 `unrecognized selector sent to class`。

`NSObject` 和 `NSProxy` 是官方中唯一可以这样的两个类，因为这两个是基类。

## 深入研究 isa

在公共头文件中 `isa` 定义是 `Class` ，但在私有头文件中 `isa` 其实是一个 `isa_t` 共用体类型：

```c++
struct objc_object {
private:
    isa_t isa;
}
```

`isa_t` 是共用体，内部使用8个字节的内存空间：

```c++
union isa_t {
    isa_t() { }
    isa_t(uintptr_t value) : bits(value) { }
    Class cls;
    uintptr_t bits; 
    struct { //这是arm64的定义
        uintptr_t nonpointer        : 1; //是否是优化过的 isa，占用1位。1表示新版本的isa指针，使用位域来存储信息；0表示旧版本普通的isa指针，直接存储Class和Meta-Class的内存地址。
        uintptr_t has_assoc         : 1; //是否有关联对象，占用1位。如果添加过关联对象，在释放时会检测是否有关联对象，所以释放会更慢。
        uintptr_t has_cxx_dtor      : 1; //是否实现了C++的析构函数（.cxx_destruct），如果没有，释放时的速度会更快。占用1位。占用33位。
        uintptr_t shiftcls          : 33;//存放类或者元类的内存地址，通过将 isa & ISA_MASK 得到。
        uintptr_t magic             : 6; //调试时用来判断对象是否完成初始化，占用6位。
        uintptr_t weakly_referenced : 1; //是否被弱引用指向过，占用1位。
        uintptr_t deallocating      : 1; //表示对象是否正在释放，占用1位。
        uintptr_t has_sidetable_rc  : 1; //表示是否将引用计数存储在 SideTable 中，占用1位。如果引用计数的值过大，在 extra_rc 无法存储，则会将引用计数存放到 SideTable 当中，置值为1。
        uintptr_t extra_rc          : 19 //存储引用计数的值，占用19位 2^19 = 524287。注意:它存储的是引用计数的值-1。如果对象的引用计数为1，则extra_rc中存储的值为0。
    };
};
```

#### initInstanceIsa

然后我们来看一下是如何初始化的 `isa` 指针的 ：

```c++
inline void objc_object::initInstanceIsa(Class cls, bool hasCxxDtor) {
    initIsa(cls, true, hasCxxDtor);
}

inline void objc_object::initIsa(Class cls, bool nonpointer, bool hasCxxDtor) { 
		isa_t newisa(0); 
  	newisa.bits = ISA_MAGIC_VALUE; //arm64下，初始化为 0x000001a000000001
		newisa.has_cxx_dtor = hasCxxDtor;      // 我们基本不会实现C++的析构函数
		newisa.shiftcls = (uintptr_t)cls >> 3; // 把类对象地址放在 isa_t 的 (3，36) 位中 
		isa = newisa;
}
```

就是把类对象的地址放到了实例对象中。

注：源码版本为objc4-779.1，[可以调试的源码下载地址](https://github.com/SunSatan/objc4-779.1CanRun)。


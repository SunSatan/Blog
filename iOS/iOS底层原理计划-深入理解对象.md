# iOS底层原理计划-深入理解对象

在 Objective-C 中的对象，主要分为3种对象：

1. instance（实例对象）
2. class（类对象）
3. meta-class（元类对象）

## instance（实例对象）

我们可以从源码 <objc4-781.2/runtime/objc.h> 可以看到实例对象的结构体的定义： 

```c++
/// Represents an instance of a class.
struct objc_object {
    Class _Nonnull isa; //只包含了一个 isa 指针
};
```

`objc_object` 中的 `isa` 指向 `objc_object` 的类对象，原因是：

```objective-c
Class object_getClass(id obj)
{
    if (obj) return obj->getIsa();
    else return Nil;
}
```

获取一个 `objc_object` 的类就是获取它的 `isa` 对象，以及`isa` 的类型 `Class` 名字出卖了它。

而 `Class` 是一个 `objc_class` 结构体的指针：

```c++
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class;
```

### id 和 NSObject

而我们常用的 `id` 就是一个 `objc_object` 的指针：

```c++
/// A pointer to an instance of a class.
typedef struct objc_object *id;
```

在 <objc4-781.2/runtime/NSObject.h> 可以看到 `NSObject` 的定义：

```objective-c
@interface NSObject <NSObject> {
    Class isa; //也只包含了一个 isa 指针
}
```

发现其实 `NSObject` 和 `objc_object` 的定义是一样的，是一个结构体在 Objective-C 和 C 中的不同表达。

这也是我们常说 `id` 本质是 `NSObject` 的原因。

## class（类对象）

我们可以从源码 <objc4-781.2/runtime/runtime.h> 可以看到之前提到的 `objc_class` 结构体的定义： 

```c++
struct objc_class {
    Class _Nonnull isa;           //元类 
    Class _Nullable super_class;  //父类
    const char * _Nonnull name;   //类名
    long version;                 //版本，默认为0
    long info;                    //信息，提供运行期使用的一些位标识
    long instance_size;           //实例变量的内存大小
    struct objc_ivar_list * _Nullable ivars; //成员变量列表
    struct objc_method_list * _Nullable * _Nullable methodLists;//方法列表
    struct objc_cache * _Nonnull cache;//调用过的方法的缓存
    struct objc_protocol_list * _Nullable protocols;//遵循的协议列表
}
```

在面向对象中，万物皆是对象。在 Objective-C 中的一个类本质也是一个对象，所以类也可以调用方法， `objc_class` 中也有 `isa` ，它指向类的元类。

`objc_class` 中 `super_class` 指向父类的 `isa`，如果类是根类，那么 `super_class` 为 `NULL` 。

`methodLists` 中存储实例方法列表（实例对象可以调用的方法列表）。

## meta-class（元类对象）

元类对象就是类对象的类，所以叫元类。

元类和类的结构体都是 `objc_class` ，但其中结构体的成员变量的作用有不一样。

元类的 `isa` 指向根类的元类，根类的元类作为所有元类的类，因此根类的元类指向自己，形成闭环。

元类的 `super_class` 指向父类的元类。

元类的 `instance_size` 默认是 40。

元类的 `methodLists` 存储类对象的类方法。

元类的 `ivars` 是不使用的，所以类方法中无法访问任何成员变量。

## 总结

任一以 `objc_class` 指针为开始的结构体，都可以视为一个 objc_object ，一个对象。

下图展示了实例、类、元类的指向和继承关系：

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/20161201152529021.png" alt="11" style="zoom:48%;" />
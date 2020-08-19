# iOS底层原理计划-内存分配和初始化

当我们有一个继承 `NSObject` 的 `Person` 类：

```objective-c
@interface Person : NSObject
@property (nonatomic, copy) NSString *name;
@end
```

问题一：当我们对实例对象只 `alloc` 未 `init` ，能否直接给其成员变量进行赋值？

```objective-c
Person *person = [Person alloc];
person.name = @"SunSatan";
```

问题二：`person1` 和 `person2` 有区别吗？

```objective-c
Person *person  = [Person alloc];
Person *person1 = [person init];
Person *person2 = [person init];
```

问题三：`person1.name` 是多少？

```objective-c
Person *person = [Person alloc];
person.name = @"SunSatan";
Person *person1 = [person init];
```

带着这三个问题，我们来探究一下iOS底层的内存分配和初始化。

在源码里，我给基类和 `NSObject` 的alloc 方法和 init 方法都加上了打印信息，以此来看内存分配和初始化调用过程。

## allow

```objective-c
Person *person = [Person alloc];
log：
objc_alloc 
callAlloc 
```

从调用过程看出，`[class alloc]` 实际调用的是基类的 `objc_alloc()` ， 而不是 `NSObject` 的 `+ (id)alloc` 。

我们来看一下 `objc_alloc()` 的源码：

```c
id objc_alloc(Class cls) { // Calls [cls alloc].
    return callAlloc(cls, true/*checkNil*/, false/*allocWithZone*/);
}

id callAlloc(Class cls, bool checkNil, bool allocWithZone=false) {
    if (slowpath(checkNil && !cls))  return nil;
    //没有自定义的 allocWithZone 方法，就调用 _objc_rootAllocWithZone
    if (fastpath(!cls->ISA()->hasCustomAWZ())) {
        return _objc_rootAllocWithZone(cls, nil);//基本执行这一步
    }
    //调用类自定义的 alloc 或 allocWithZone: 方法
    if (allocWithZone) {
        return ((id(*)(id, SEL, struct _NSZone *))objc_msgSend)(cls, @selector(allocWithZone:), nil);
    }
    return ((id(*)(id, SEL))objc_msgSend)(cls, @selector(alloc));
}
```

我们经常会重写 `init` ，但基本不会重写 `alloc` ，所以都是最终都是调用了 `_objc_rootAllocWithZone()` 方法：

```c
id _objc_rootAllocWithZone(Class cls, malloc_zone_t *zone __unused) {
    return _class_createInstanceFromZone(cls, 0, nil, OBJECT_CONSTRUCT_CALL_BADALLOC);// objc2以后都忽略 zone
}

id _class_createInstanceFromZone(Class cls, size_t extraBytes, void *zone, int construct_flags = OBJECT_CONSTRUCT_NONE, bool cxxConstruct = true, size_t *outAllocatedSize = nil) {
		...
    size_t size;// 计算实例变量的内存大小
    size = cls->instanceSize(extraBytes);//extraBytes = 0
    ...
    id obj; 
    if (zone) { //zone = nil
        obj = (id)malloc_zone_calloc((malloc_zone_t *)zone, 1, size);
    } else {
        obj = (id)calloc(1, size); //执行这一步，分配内存
    }
    ...
    if (!zone && fast) {  //fast 指是不是优化过的 isa，现在基本都是优化的 isa 了
        obj->initInstanceIsa(cls, hasCxxDtor); //所以执行这一步，初始化实例的isa
    } else {
        obj->initIsa(cls);
    }
    if (fastpath(!hasCxxCtor)) { // 一般 hasCxxCtor = false
        return obj;   //然后就返回
    }
		...
}
```

可以看到 `_class_createInstanceFromZone()` 为实例对象计算了内存大小，接着分配内存，初始化实力对象的  `isa` ，最后返回一个完整的实例对象。

所以我们现在知道问题一的答案了，多数情况下是可以的，除了抽象工厂模式创建的类簇对象，需要初始化后才能确定具体的类型。

### instanceSize

我们看一下是如何为实例对象计算内存大小的：

```c
size_t instanceSize(size_t extraBytes) const {
    // objc_class 缓存了 Instance Size
    if (fastpath(cache.hasFastInstanceSize(extraBytes))) {
        return cache.fastInstanceSize(extraBytes);
    }
    //我调试的时候，下面一次都没执行过，不知道什么情况下会执行
    size_t size = alignedInstanceSize() + extraBytes;
    if (size < 16) size = 16;//至少也会分配16字节
    return size;
}
```

通常，类对象已经缓存了实例对象的内存大小，直接取出来，然后进行了16字节对齐：

```c++
size_t align16(size_t x) { //16字节对齐算法
    return (x + size_t(15)) & ~size_t(15);
}

size_t fastInstanceSize(size_t extra) const {
    size_t size = _flags & FAST_CACHE_ALLOC_MASK;// 取出缓存在 _flags 中的内存大小
    // 移除在 setFastInstanceSize 里加的 FAST_CACHE_ALLOC_DELTA16 = 8
    return align16(size + extra - FAST_CACHE_ALLOC_DELTA16);// extra等于0，不应管
}
```

在类对象没缓存的情况下，取出未对齐的内存大小，进行8字节对齐：

```c
#define WORD_MASK 7UL
uint32_t word_align(uint32_t x) { //8字节对齐算法
    return (x + WORD_MASK) & ~WORD_MASK;
}

uint32_t alignedInstanceSize() const {
    return word_align(unalignedInstanceSize());//取未对齐的内存大小，进行字节对齐
}
```

我们可以来验证一下，在 `_class_createInstanceFromZone()` 里把计算好的 `size` 打印出来：

```c
if (strcmp(cls->demangledName(), "Person") == 0) {
    printf("- %s final instanceSize:%zu \n", "Person", size);
}
```

首先继续使用之前的 `Person` 类，删除原有的 `name` 属性，然后创建一个 `Person` 的实例对象，运行后打印如下：

```objective-c
- Person final instanceSize:16 
```

**最少会分配16字节的内存给实例对象，哪怕实例对象只使用了8字节内存。**

接着给 `Person` 类添加 `name` 和 `age` 属性：

```objective-c
@interface Person : NSObject
//isa  8字节
@property (nonatomic, copy) NSString *name; //8字节
@property (nonatomic, assign) int age;      //1字节
@end
```

此时 `Person` 使用的内存大小为17字节小于24字节，如果使用8字节对齐则会分配24字节，如果使用16字节对齐则会分配32字节。

运行后打印如下：

```objective-c
- Person final instanceSize:32 
```

**说明底层确实使用的是16字节对齐。**

## allocWithZone

```objective-c
Person *person = [Person allocWithZone:nil];
log：
objc_allocWithZone 
callAlloc
```

当 `zone` 为 `nil` 时，`[class allocWithZone:nil]` 实际调用的也是基类的 `objc_allocWithZone()` ，最终调用 `callAlloc()`：

```objective-c
id objc_allocWithZone(Class cls) { // Calls [cls allocWithZone:nil].
    return callAlloc(cls, true/*checkNil*/, true/*allocWithZone*/);
}
```

当 `zone` 不为 `nil` 时，就会调用`NSObject` 中 `+ (id)allocWithZone:` 方法

```objective-c
Person *person = [Person allocWithZone:NSDefaultMallocZone()];
log：
+ (id)allocWithZone 
```

而 `+ (id)allocWithZone:` 直接越过 `callAlloc()`，调用 `_objc_rootAllocWithZone()` ，把 `zone` 传进去：

```objective-c
+ (id)allocWithZone:(struct _NSZone *)zone {
    return _objc_rootAllocWithZone(self, (malloc_zone_t *)zone);
}
```

但我们通过之前的分析，知道 `_objc_rootAllocWithZone()` 是忽略了 `zone` 的，所以 `alloc` 和 `allocWithZone` 本质是一样的。

## init 

```objective-c
person = [person init];
log：
- (id)init
_objc_rootInit
```

我们来看看 `- (id)init` 方法和 `_objc_rootInit()` 的源码：

```objective-c
+ (id)init {
    return (id)self;
}

- (id)init {
    return _objc_rootInit(self);
}

id _objc_rootInit(id obj) {
    return obj;
}
```

其实 `init` 就直接把调用者返回来了，什么都没做。因为 `init` 使用了工厂方法，让你为自己的类进行初始化的工作。

因此问题二和问题三的答案就显而易见了。

## new

```objective-c
Person *person = [Person new];
log：
objc_opt_new 
callAlloc 
- (id)init 
_objc_rootInit
```

`[NSObject new]` 实际执行的也是基类的实现 `objc_opt_new()`，而不是 `+ (id)new ` ：

```c++
id objc_opt_new(Class cls) {// Calls [cls new]
    if (fastpath(cls && !cls->ISA()->hasCustomCore())) {
        return [callAlloc(cls, false/*checkNil*/, true/*allocWithZone*/) init];
    }
    return ((id(*)(id, SEL))objc_msgSend)(cls, @selector(new));
}
```

`new` 或者 `objc_opt_new()` 就是把 `allow` 和 `init` 合成为了一步，并没有做其他的事。

因此，如果我们不需要使用特殊的 `init` 来创建，我们用 `new` 来创建实例对象，代码更少更简洁。

## 疑问

通过研究，我反而产生了一个疑问：`+ (id)alloc` 方法一直都没有调用，到底有什么用呢？

```objective-c
+ (id)alloc {
    return _objc_rootAlloc(self);
}

id _objc_rootAlloc(Class cls) {
    return callAlloc(cls, false/*checkNil*/, true/*allocWithZone*/);
}
```

根据我的理解和测试，应该是为了给我们重写的 `+ (id)alloc ` 里面调用 `[super alloc]` 的。

**注1：本文中出现的源码，我已经把其中无关的预编译、断言、代码段等删除了。**

**注2：源码版本为objc4-779.1，[一个可以调试的源码的下载地址](https://github.com/SunSatan/objc4-779.1CanRun)。**


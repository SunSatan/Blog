# iOS底层原理计划-内存分配和初始化

## 现象



## 探究iOS的内存分配和初始化

### allow

我们所有的类都调用都是 `NSObject` 的 `+ (id)alloc` 方法：

```objective-c
+ (id)alloc {
    return _objc_rootAlloc(self);
}
```

我们接着进入 `_objc_rootAlloc()` 方法里面：

```c++
id _objc_rootAlloc(Class cls)
{
    return callAlloc(cls, false/*checkNil*/, true/*allocWithZone*/);
}
```

我们接着进入 `callAlloc()` 方法里面：

```c++
// Call [cls alloc] or [cls allocWithZone:nil], with appropriate shortcutting optimizations.
static ALWAYS_INLINE id callAlloc(Class cls, bool checkNil, bool allocWithZone=false)
{
    if (slowpath(checkNil && !cls)) 
      return nil;
    //没有自定义的 allocWithZone 方法，就调用 _objc_rootAllocWithZone
    if (fastpath(!cls->ISA()->hasCustomAWZ())) {
        return _objc_rootAllocWithZone(cls, nil);
    }
    
    //调用类自定义的 alloc 或 allocWithZone: 方法
    if (allocWithZone) {
        return ((id(*)(id, SEL, struct _NSZone *))objc_msgSend)(cls, @selector(allocWithZone:), nil);
    }
    return ((id(*)(id, SEL))objc_msgSend)(cls, @selector(alloc));
}
```

一般我们会重写 `init` ，但不会重写 `alloc` ，所以我们来看 `_objc_rootAllocWithZone()` 方法：

```c++
NEVER_INLINE id _objc_rootAllocWithZone(Class cls, malloc_zone_t *zone __unused)
{
    // allocWithZone under __OBJC2__ ignores the zone parameter
    return _class_createInstanceFromZone(cls, 0, nil, OBJECT_CONSTRUCT_CALL_BADALLOC);
}
```

又是一个更底层的调用，继续进入 `_class_createInstanceFromZone()` 内部：

```c++
static ALWAYS_INLINE id
_class_createInstanceFromZone(Class cls, size_t extraBytes, void *zone,
                              int construct_flags = OBJECT_CONSTRUCT_NONE,
                              bool cxxConstruct = true,
                              size_t *outAllocatedSize = nil)
{
    ASSERT(cls->isRealized());
    // Read class's info bits all at once for performance
    bool hasCxxCtor = cxxConstruct && cls->hasCxxCtor();
    bool hasCxxDtor = cls->hasCxxDtor();
    bool fast = cls->canAllocNonpointer();
  
    
  	// 计算实例变量的内存大小
    size_t size;
    size = cls->instanceSize(extraBytes);//extraBytes 都是传入 0
  
    if (outAllocatedSize) 
      *outAllocatedSize = size;

    id obj;
    if (zone) {
        obj = (id)malloc_zone_calloc((malloc_zone_t *)zone, 1, size);
    } else {
        obj = (id)calloc(1, size); //执行这一步，分配内存
    }
    if (slowpath(!obj)) {
        if (construct_flags & OBJECT_CONSTRUCT_CALL_BADALLOC) {
            return _objc_callBadAllocHandler(cls);
        }
        return nil;
    }

    if (!zone && fast) {
        obj->initInstanceIsa(cls, hasCxxDtor);
    } else {
        // Use raw pointer isa on the assumption that they might be
        // doing something weird with the zone or RR.
        obj->initIsa(cls);
    }

    if (fastpath(!hasCxxCtor)) {
        return obj;
    }

    construct_flags |= OBJECT_CONSTRUCT_FREE_ONFAILURE;
    return object_cxxConstructFromClass(obj, cls, construct_flags);
}
```



```c++
size_t instanceSize(size_t extraBytes) const {
        if (fastpath(cache.hasFastInstanceSize(extraBytes))) {
            return cache.fastInstanceSize(extraBytes);
        }

        size_t size = alignedInstanceSize() + extraBytes;
        // 
        if (size < 16) size = 16;
        return size;
    }
```



### allocWithZone

在 `NSObject` 中 `+ (id)allocWithZone:` 方法：

```objective-c
+ (id)allocWithZone:(struct _NSZone *)zone {
    return _objc_rootAllocWithZone(self, (malloc_zone_t *)zone);
}
```

`_objc_rootAllocWithZone()` 我们已经在上文花了很大的篇幅进行分析了。

`allocWithZone` 和 `alloc` 的区别：

### init 

我们再来看看 `NSObject` 的 `- (id)init` 方法做了什么事：

```objective-c
+ (id)init {
    return (id)self;
}

- (id)init {
    return _objc_rootInit(self);
}
```

类方法返回 `self`，因为类对象不需要初始化。

实例方法调用了 `_objc_rootInit(self)` ，那我们继续来看：

```c++
id _objc_rootInit(id obj) {
    return obj;
}
```

`_objc_rootInit(self)` 直接把 `self` 又返回了回来，本质上就是什么都没做。

就是让你为自己创建的类写 init，因为这是你的事。

### new

我们经常使用创建实例对象，还有一个 `+ (id)new` 方法：

```objective-c
+ (id)new {
    return [callAlloc(self, false/*checkNil*/) init];
}
```

其实就是把 `allow` 和 `init` 合成了一步，并没有做其他的事。

因此，如果类不需要使用特殊的 `init` 来创建，我们用 new 来创建实例对象，代码更少更简洁。
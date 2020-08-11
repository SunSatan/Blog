# iOS底层原理计划-消息发送和转发机制

在 Objective-C 中的方法调用，其实都是消息发送的过程，Objective-C 采用**动态绑定机制**，所以所要调用的方法直到运行期才能确定。

xcrun -sdk iphoneos  clang -arch arm64 -rewrite-objc ViewController -o ViewController-arm64.cpp

都是转换为 `objc_msgSend()` 函数的调用。

```objective-c
((void (*)(id, SEL))(void *)objc_msgSend)((id)self, sel_registerName("test"));
```

消息发送和转发流程可以概括为：消息发送（Messaging）是 Runtime 通过 selector 快速查找 IMP 的过程，有了函数指针就可以执行对应的方法实现；消息转发（Message Forwarding）是在查找 IMP 失败后执行一系列转发流程的慢速通道，如果不作转发处理，则会打日志和抛出异常。

```objective-c
id objc_msgSend(id self, SEL _cmd, ...) {
  Class class = object_getClass(self);
  IMP imp = class_getMethodImplementation(class, _cmd);
  return imp ? imp(self, _cmd, ...) : 0;
}
```
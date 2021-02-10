# iOS开发-实例对象的生命周期 #

实例对象的生命周期（Object life cycle）是指实例对象从创建到销毁的整个过程。

## Objective-C ##

### NSObject ###

在 Objective-C 中，所有类都是继承自 `NSObject`，因此首先需要了解 `NSObject` 的实例对象的生命周期。

实例对象的出现一般有两个途径：

- 手动创建实例对象。
- 从已归档的二进制数据流中解档出实例对象。

手动创建实例对象需的生命周期： `alloc` -> `allocWithZone:` -> `init` -> 操作 -> 对象释放 -> `dealloc`。

解档出实例对象的生命周期： `alloc` -> `allocWithZone:` -> `initWithCoder:` -> 操作 -> 对象释放 -> `dealloc`。

### UIView ###

我们常用类多是继承自 `UIView` ，而 `UIView` 的实例对象的生命周期与 `NSObject` 有稍许不同。

手动创建 `UIView` 的生命周期：`alloc` -> `allocWithZone:` -> `init` -> `initWithFrame:` -> 操作 -> 对象释放 -> `dealloc`。

从 xib / storyboard 中被创建 `UIView`  的生命周期： `alloc` -> `allocWithZone:` -> `initWithCoder:` -> `awakeFromNib`  -> 操作 -> 对象释放 -> `dealloc`。

因此在我们通常需要将自定义控件的初始化操作抽离出来，分别在 `initWithFrame:` 和 `initWithCoder:` 调用，来保证无论何种方式创建我们的自定义控件都不会出现问题。

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20201226233954700.png" alt="image-20201226233954700" style="zoom: 67%;" />

注：`dealloc` 在对象释放以后才会调用，所以在 `dealloc`  调用 `self` 需要谨慎。



## Swift ##

在 Swift 中。
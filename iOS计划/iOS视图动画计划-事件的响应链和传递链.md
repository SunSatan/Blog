# iOS视图计划-事件的响应链和传递链

### 前言
当我们点击屏幕的时候，屏幕就会产生了一个点击事件，那我们的程序是如何知道这个点击事件应该由**谁**来处理？
### 响应者
在iOS中，能够响应事件的就是响应者，而所有响应者都是 `UIResponser` 的子类，`UIView`、 `UIButton` 、 `UIControl`、 `UIWindow`、`UIViewController`、`AppDelegate`、`UIApplication`以及它们的子类。

`UIResponder ` 声明了各种点击事件的处理，比如点击，按压，移动等等。

### 响应链
响应链就是响应者都连接一起的一个链条的层级关系，说是链条其实更类似树结构。

这个链条从程序开始运行时就建立并不断将响应者链接进来。

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/20200803023949155.png" alt="img" style="zoom: 67%;" />

我们都知道程序运行后，`UIApplication` 会生成一个单例，并与 `AppDelegate` 进行关联。而 `AppDelegate` 就作为整个响应链的根建立存在，接着 `UIApplication `的单例就会作为响应者链接在根上：

```objective-c
 [UIApplication sharedApplication].nextResponser = AppDelegate
```

当任何一个 `UIWindow` 被创建时，`UIWindow` 都会自动链接在 `UIApplication` 的单例上，即把 `UIWindow` 的`nextResponser `设置为 `UIApplication` 的单例。

当 `UIWindow` 设置 `rootViewController` 时，`rootViewController` 就会链接在 `UIWindow` 上。

`UIViewController` 初始化 `loadView` 时，`UIViewController` 的 `view` 就会链接在 `UIViewController` 上。

`addSubView` 的操作过程中，`subView `的 `nextResponser` 会被设置为 `superView`。

举例验证环节：

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.window.rootViewController = [[ViewController alloc] init];
    [self.window makeKeyAndVisible];
    return YES;
}
//ViewController.m
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    UIView *view = [[UIView alloc] init];
    [self.view addSubview:view];
    
    UILabel *label = [[UILabel alloc] init];
    [view addSubview:label];
    [self logOutResponderChain:label];
}

- (void)logOutResponderChain:(UIResponder *)responder {
    UIResponder *nextResponder = responder.nextResponder;
    NSLog(@"%@ -> ", NSStringFromClass([responder class]));
    while (nextResponder) {
        NSLog(@"%@ -> ", NSStringFromClass([nextResponder class]));
        nextResponder = nextResponder.nextResponder;
    }
    NSLog(@"*");
}
```
打印结果如下：
```objectivec
UILabel -> UIView -> UIView -> ViewController -> UIDropShadowView -> UITransitionView -> 
UIWindow -> UIWindowScene -> UIApplication -> AppDelegate -> *
```
### 传递链
通过上文，我们已经知道了响应链是如何建立的，而建立响应链就是为了让事件能找到对应的处理者，而找到处理者的过程称之为事件的传递链。

**这里先给出结论：**

1. 当用户点击屏幕触发事件，系统硬件进程会获取到这个事件，将事件简单处理封装后存到系统中，系统接着将事件转交到 `UIApplication` 管理的事件队列中。（这一部分涉及RunLoop和端口通信）
2. `UIApplication` 会从事件队列中取出最前面的事件，并将事件顺着响应链分发下去，寻找合适的控件进行处理。
3. 根据响应链，`UIApplication ` 会先发送事件给主窗口 `keyWindow`。
4. `keyWindow` 再根据响应链逐级分发下去，直到找到合适的处理控件为止。
5. 找到合适的处理控件后，`keyWindow ` 就会调用该控件中合适的事件方法来处理事件。
6. 如果找不到合适的处理控件，该事件就会被废弃。

**那么怎么判断哪个控件是事件的最合适的处理者？**

1. 控件肯定要显示在屏幕上，用户要能看见才会去触发事件。
2. 控件可以响应点击事件。
3. 点击事件发生在控件范围内。
4. 该控件没有其他子控件可以处理该事件。

**事件是怎么逐级分发下去的？**

通过调用下面两个UIView的方法进行事件的分发。

```objectivec
// 判断事件触发点是否在这个View内部
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event；
// 根据上一个方法的结果，遍历View的subViews
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event；
```
**这两个方法是如何进行传递的，进行验证环节：**

我首先创建三个继承 `UIView` 的子类 `AView`、`BView`、`CView`，然后都重写`pointInside:withEvent:`、`hitTest:withEvent:` 和 `touchesBegan:withEvent:` 方法，进行打印。
```objectivec
//注：AView、BViewh和CView都一样，就不重复粘贴了。
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    NSLog(@"AView --- hitTest");
    UIView * view = [super hitTest:point withEvent:event];
    NSLog(@"AView --- hitTest --- return %@", view.class);
    return view;
}

- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    NSLog(@"AView --- pointInside");
    return [super pointInside:point withEvent:event];
}

- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    NSLog(@"AView --- touchesBegan");
}
```
然后添加到UIViewController的view上。

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor orangeColor];
    
    AView *a = [[AView alloc] initWithFrame:CGRectMake(100, 100, 300, 300)];
    a.backgroundColor = [UIColor blueColor];
    [self.view addSubview:a];
    
    BView *b = [[BView alloc] initWithFrame:CGRectMake(100, 100, 200, 200)];
    b.backgroundColor = [UIColor greenColor];
    [a addSubview:b];
    
    CView *c = [[CView alloc] initWithFrame:CGRectMake(100, 100, 100, 100)];
    c.backgroundColor = [UIColor grayColor];
    [b addSubview:c];
}
```
<center><img src="https://img-blog.csdnimg.cn/20200803135830348.png" width="300" height="533"></center>

首先我们在`CView`的`pointInside:withEvent:`中打个断点，然后点击`CView`：

<center><img src="https://img-blog.csdnimg.cn/20200803152510999.png" width="400" height="250"></center>

查看调用栈，`pointInside:withEvent:`是在`hitTest:withEvent:`中调用的，只有返回`Yes`才会去遍历子控件。

接着放开断点，事件传递链打印如下：

```objectivec
AView --- hitTest
AView --- pointInside --- Yes
BView --- hitTest
BView --- pointInside --- Yes
CView --- hitTest
CView --- pointInside --- Yes
CView --- hitTest --- return CView
BView --- hitTest --- return CView
AView --- hitTest --- return CView
CView --- touchesBegan
```
因为`CView`没有子控件，且点击在`CView`上，所以返回`CView`作为事件处理者，来调用`touchesBegan:withEvent:`。

接着点击`BView`但不在`CView`上，事件传递链打印如下：
```objectivec
AView --- hitTest
AView --- pointInside --- Yes
BView --- hitTest
BView --- pointInside --- Yes
CView --- hitTest
CView --- pointInside --- No
CView --- hitTest --- return (null)
BView --- hitTest --- return BView
AView --- hitTest --- return BView
BView --- touchesBegan
```
因为点击不在`CView`上所以返回了`(null)`，而点击又在`BView`上，所以返回`BView`作为事件处理者，来调用`touchesBegan:withEvent:`。

通过上面的测试，我们已经证明了判断条件（3）点击事件发生在控件范围内和（4）该控件没有子控件可以处理事件的正确性，然后来看一下（1）和（2）。

对`CView`分别进行设置然后测试：
```objectivec
c.userInteractionEnabled = NO;
```
```objectivec
c.hidden = YES;
```
```objectivec
c.alpha = 0;
```
三次事件，打印结果一致如下：

```objectivec
AView --- hitTest
AView --- pointInside --- Yes
BView --- hitTest
BView --- pointInside --- Yes
CView --- hitTest
CView --- hitTest --- return (null)
BView --- hitTest --- return BView
AView --- hitTest --- return BView
BView --- touchesBegan
```
当`CView`未显示在屏幕上或不能响应点击事件时，`hitTest:withEvent:`都不调用`pointInside:withEvent:`直接返回了`(null)`。虽然点击在`CView`上，但`CView`不能响应事件，因此由`CView`的上一级`BView`来处理。

经过上面的验证过程，我们可以得到UIView中`hitTest:withEvent:`大概的内部实现：
```objectivec
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    if (self.alpha == 0  ||
        self.hidden == YES ||
        self.userInteractionEnabled == NO) {
        return nil;
    }

    if (![self pointInside:point withEvent:event]) {
        return nil;
    }

    UIView *eventView = nil;
    for (UIView *subView in self.subviews) {
        //以子视图为参考系转换坐标点，漏这一步 pointInside:withEvent:会出错，事件就传递不下去了。
        CGPoint subPoint = [self convertPoint:point toView:subView];
        eventView = [subView hitTest:subPoint withEvent:event];
    }
    if (!eventView) {
        eventView = self;
    }

    return eventView;
}
```
### 传递链的简单总结
用户产生触摸事件 -> 事件进入 `UIApplication` 事件队列 -> `UIApplication` 分发事件给 `keyWindow` -> `[UIWindow hitTest:withEvent:]` -> `[UIView hitTest:withEvent:]` -> ... -> `[UIView hitTest:withEvent:]`  -> 返回最合适的`view`给`keyWindow` -> `[UIWindow _sendTouchesForEvent:]`  -> `[UIView touchesBegan:withEvent:]`。
### 传递链面试题
**问：父视图设置为不可点击，子视图为什么也不可以点击？**

答：当父视图设置为不可点击，传递链在父视图就被截至返回为`(null)`了，都传递不到子视图。

**问：事件传递链和事件响应链区别？**

答：事件传递链是从父控件到子控件传递，从上到下。事件响应链是从子控件到父控件进行响应、链接，从下到上。

**问：子视图在父视图之外显示的区域，点击是否有效？**

答：虽然默认会显示，但是点击是无效的，父视图的`pointInside:withEvent:`判断通不过。

### 传递链应用
#### 实战应用：截至传递
如果我们不想让视图响应事件，除了`userInteractionEnabled = NO`，还可以重载`pointInside:withEvent:`方法，返回NO就行了。

```objectivec
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event{
    return NO;
}
```
#### 实战应用：子视图和父视图同时处理一个事件
子视图需要重载`touch`方法来处理事件，只需要再调用`[super  touch]`将事件传递给父视图即可：
```objectivec
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //子视图处理该事件
    NSLog(@"%s",__func__);
    //再调用 super 让父视图也处理该事件
    [super touchesBegan:touches withEvent:event];
}
```
#### 实战应用：隔层传递
假设`vc.view`上添加`AView`，`AView`添加了`BView`，`BView`又添加了`CView`。

点击`CView`可以响应处理事件，点击`BView`却不响应，而`AView`却可以响应处理事件，跳过中间层进行响应事件，也就是隔层传递。

```objectivec
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event{
    
    CGRect frame = CGRectMake(0, 0, self.frame.size.width, self.frame.size.height);
    BOOL value = (CGRectContainsPoint(frame, point));
    NSArray *views = [self subviews];
    for (UIView *subview in views) {
        value = (CGRectContainsPoint(subview.frame, point));
        if (value) {
            return value;
        }
    }
    return NO;
}
```
#### 实战应用：扩大控件的响应范围
我们可以重载`pointInside:withEvent:`方法，将控件的判断范围变大就可以了：
```objectivec
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    CGRect relativeFrame = self.bounds;
    UIEdgeInsets hitTestEdgeInsets = UIEdgeInsetsMake(-15, -15, -15, -15);
    CGRect hitFrame = UIEdgeInsetsInsetRect(relativeFrame, hitTestEdgeInsets);
    return CGRectContainsPoint(hitFrame, point);
}
```
#### 实战应用：深层级 View 之间的通信
假设`vc.view`上添加`AView`，`AView`添加了`BView`，`BView`又添加了`CView`。

当`CView`有一个事件需要`vc`来处理，这个时候如果用 block、delegate、Notification 都会比较麻烦，这个时候就可以通过响应者链，将消息传递上去。

首先我们为 `UIResponder` 写个分类方法，类似Router方法，让整个响应链传递这个方法，`CView` 只需要调用该方法，让控制器去重写该方法就可以和 `CView `进行通信了。

```objectivec
//UIResponder 分类实现
/**
 发送一个路由器消息, 对eventName感兴趣的 UIResponsder 可以对消息进行处理
 @param eventName 发生的事件名称。
 @param userInfo 传递消息时, 携带的数据。
 */
- (void)routerEventWithName:(NSString *)eventName userInfo:(NSObject *)userInfo {
    [[self nextResponder] routerEventWithName:eventName userInfo:userInfo];
}

//CView调用
[self routerEventWithName:@"CViewEvent" userInfo:nil];

//控制器重载
- (void)routerEventWithName:(NSString *)eventName userInfo:(NSObject *)userInfo {
    NSLog(@"%s eventName:%@",__func__,eventName);
}
```
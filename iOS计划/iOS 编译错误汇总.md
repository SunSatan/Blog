# iOS 编译错误汇总 #

### 1、Cannot synthesize weak property in file using manual reference counting ###

（不能使用手动引用计数合成文件中的弱属性）。

#### 原因 ####

设置中弱引用选项没有开启。

#### 解决方法 ####

在 TARGETS 的 Build Settings 找到 `Weak References in Manual Retain Release` （弱引用手动计数释放）选项改为 YES 。


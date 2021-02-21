# iOS开发-沙盒机制

## 沙盒机制原理

iOS 为了保证安全和隐私，使用沙盒（sandbox）机制来管理每个 App 的数据和文件。

iOS App 在安装时就会创建只属于自己的沙盒空间，每个 App 的沙盒是相互独立的，每个 App 只可读写自己的沙盒，无法访问其他 App 的沙盒。

我们在开发中常常需要数据缓存、存取文件、解档归档，因此我们有必要熟悉沙盒目录及其存储作用。

## 沙盒目录

沙盒的根目录可以直接通过 `NSHomeDirectory()` 获取。

根目录下默认有四个文件目录：Documents、Library、tmp、SystemData。

这些文件路径可以使用 `NSSearchPathForDirectoriesInDomains()` 获取：

```objective-c
NSArray<NSString *> *NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory directory, NSSearchPathDomainMask domainMask, BOOL expandTilde);
// 返回值为路径数组，但是数组中只有一个路径。
// 参数1：directory，表示需要查找的目标文件夹。
// 参数2：domainMask，表示查找路径，iOS 中只能使用 NSUserDomainMask（用户的主目录）。
// 参数3：expandTilde，表示返回路径是否展开，展开就是绝对路径。
```

也可以直接使用根目录拼接获取：

```objective-c
NSString *path = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents"];
```

### 1、Documents/

Documents 用于存放需要长久保存的**重要**数据和文件、用户生成的数据和文件，注意不要存放太大的文件。

 iTunes/iCloud 会自动备份 Documents 下的文件，使用共享文件时，可以共享 Documents 文件目录。

```objective-c
NSString *path1 = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject
NSString *path2 = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents"];
```

### 2、Library/

#### 2.1、Library/Caches

Caches 用于存放可缓存的数据和文件，是可以重新下载得到的数据和文件，比如网络请求数据、下载的图片、视频、音频资源等。

 iTunes/iCloud 不会备份 Caches 下的文件，当设备存储空间不足时，系统会删除 tmp 下的文件。

```objective-c
NSString *path1 = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES).lastObject
NSString *path2 = [NSHomeDirectory() stringByAppendingPathComponent:@"Library/Caches"];
```

#### 2.2、Library/Preferences

Preferences 主要存放用户对 App 的所有偏好设置，比如用户的账号、应用的设置、主题等。

NSUserDefaults 的 <Bundle IDentifier>.plist 就存放在 Preferences 里。

 iTunes/iCloud 会自动备份 Preferences 下的文件。

系统未提供直接访问的路径，需要拼接路径：

```objective-c
NSString *path = [NSHomeDirectory() stringByAppendingPathComponent:@"Library/Preferences"];
```

#### 2.3、Library/SplashBoard

SplashBoard 存放着 App 启动页的缓存数据，如果需要动态切换启动页或新换的启动页不显示，可以尝试删除 `Library/SplashBoard/Snapshots` 文件夹。

 iTunes/iCloud 不会备份 SplashBoard 下的文件。

系统未提供直接访问的路径，需要拼接路径：

```objective-c
NSString *path = [NSHomeDirectory() stringByAppendingPathComponent:@"Library/SplashBoard"];
```

### 3、tmp/

tmp 用于保存临时文件，是之后不会再使用的文件，临时文件使用完成后需要尽快清理，避免浪费存储空间。

iTunes/iCloud 不会备份 tmp 下的文件，当设备存储空间不足时、重启设备时，系统会删除 tmp 下的文件。

```objective-c
NSString *path1 = NSTemporaryDirectory();
NSString *path2 = [NSHomeDirectory() stringByAppendingPathComponent:@"tmp"];
```

### 4、SystemData/

SystemData 望文生义猜测是存放系统操作数据的目录，但是具体作用未知，在不明白其存储规则时不建议使用。

系统没有提供可直接访问的路径，只能通过根目录拼接来获取路径。


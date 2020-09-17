# CocoaPods 使用

CocoPods 安装好以后，使用时先 cd 到项目文件路径下，然后使用 CocoaPods 命令进行操作。

##### 1、为项目创建 Podfile 文件：

```
pod init
```

##### 2、编辑 Podfile 文件，添加配置和第三方库：

```
platform :ios, '10.0' # 指定第三方库所支持 iOS 的最低版本

inhibit_all_warnings! # 忽略所有警告

# 指定使用 cocopods 的源，一般不需要
# source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'

target 'StudyObject' do # 声明当前 Podfile 只对 StudyObject 有效。

  use_frameworks!   # 第三方库将使用动态库，推荐使用，而 swift 项目必须使用。
  
  pod 'AFNetworking' # 不指定下载第三方库的版本，会保持最新版本
  pod 'SDWebImage', '3.7.1'  # 指定下载第三方库的版本只为 3.7.1
  pod 'MJRefresh', '~> 3.5.0' # 指定下载第三方库的版本为 [3.5.0, 3.6.0) 之间
  pod 'AFNetworking', '> 3.5.0'  # 不解释
  pod 'AFNetworking', '< 3.5.0'  # 不解释
  pod 'AFNetworking', '>= 3.5.0' # 不解释
  pod 'AFNetworking', '<= 3.5.0' # 不解释
  
end
```

##### 3、安装第三方库，第一次安装会慢一些：

```
pod install
```

未指定版本的第三方库，会自动更新到最新的版本。



```
pod install --verbose --no-repo-update
```

该命令只安装新添加的库，已更新的库忽略

```
pod update 库名 --verbose --no-repo-update
```


该命令只更新指定的库，其它库忽略


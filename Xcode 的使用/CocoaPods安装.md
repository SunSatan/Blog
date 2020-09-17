# CocoaPods 安装

### 前言

每次换电脑就要装一次 CocoaPods，所以想着总结一下，方便自己的同时也供大家学习。

以下安装方法，本人在 MacOS 10.15.2 下安装成功，无任何错误，中间因网络原因可能会导致下载中断出错，可以通过科学上网或其他网络手段解决。

### CocoaPods 简介

CocoaPods 是一个用 Ruby 编译、负责管理 iOS 项目中第三方开源库的工具。

CocoaPods 能让我们集中的、统一管理第三方开源库，并且自动为三方库添加依赖。

### CocoaPods 安装

MacOS 自带 Ruby 环境，而安装 CocoaPods 需要 Ruby 2.2.2 及以上的，所以我们首先打开终端，查看 Ruby 的版本。

```
ruby -v
```

因为 MacOS 10.15.2 默认是最新的 ruby 2.6.3，所以我们不需要升级 Ruby 环境。

然后我们需要更新 gem，并且更换 gem 为国内的镜像。

```
sudo gem update --system

gem sources --remove https://rubygems.org/

gem sources --add https://gems.ruby-china.com/
```

可以使用下面的命令查看 gem 镜像 是否为 `https://gems.ruby-china.com/` 。

```
gem sources -l
```

环境搭建完成，开始安装 CocoaPods。

```
sudo gem install -n /usr/local/bin cocoapods
```

下载完成，开始安装本地库。

```
pod setup
```

MacOS 10.15 一下的系统会开始下载 github 镜像，这就只能听天命了。

```
Setting up CocoaPods master repo
$ /usr/bin/git clone https://github.com/CocoaPods/Specs.git master --progress
Cloning into 'master'...
remote: Counting objects: 1879515, done.        
remote: Compressing objects: 100% (321/321), done.        
Receiving objects:  21% (404525/1879515), 73.70 MiB | 22.00 KiB/
```

而 MacOS 10.15 系统会直接显示 `Setup completed` ，但本地库其实并没有安装，所以我们还需要手动安装，推荐使用国内镜像。

```
//github镜像，速度慢
git clone https://github.com/CocoaPods/Specs.git ~/.cocoapods/repos/trunk

//国内镜像，速度快
git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git  ~/.cocoapods/repos/trunk
```

安装完成后，查看是否安装成功，第一次使用等一会。

```
pod search AFNetworking
```

出现以下信息表示安装成功。

```
-> AFNetworking (4.0.1)
   A delightful networking framework for Apple platforms.
   pod 'AFNetworking', '~> 4.0.1'
   - Homepage: https://github.com/AFNetworking/AFNetworking
   - Source:   https://github.com/AFNetworking/AFNetworking.git
   - Versions: 4.0.1, 4.0.0, 3.2.1, 3.2.0, 3.1.0, 3.0.4, 3.0.3, 3.0.2, 3.0.1,
   3.0.0, 3.0.0-beta.3, 3.0.0-beta.2, 3.0.0-beta.1, 2.7.0, 2.6.3, 2.6.2, 2.6.1,
   2.6.0, 2.5.4, 2.5.3, 2.5.2, 2.5.1, 2.5.0, 2.4.1, 2.4.0, 2.3.1, 2.3.0, 2.2.4,
   2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0, 2.0.3, 2.0.2, 2.0.1, 2.0.0, 2.0.0-RC3,
   2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3, 1.3.2, 1.3.1, 1.3.0, 1.2.1, 1.2.0, 1.1.0,
   1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1, 0.10.1, 0.10.0, 0.9.2, 0.9.1, 0.9.0,
   0.7.0, 0.5.1 [trunk repo]
   - Subspecs:
     - AFNetworking/Serialization (4.0.1)
     - AFNetworking/Security (4.0.1)
     - AFNetworking/Reachability (4.0.1)
     - AFNetworking/NSURLSession (4.0.1)
     - AFNetworking/UIKit (4.0.1)
```


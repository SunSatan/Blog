# Mac 启动台里 App 图标重复，怎么办？

我昨天在 Mac App Store 上更新了两个应用程序，然后在启动台（ Launchpad ）里就出现两对两个一样的 App 图标。

这可怎么办呢？我的启动台（ Launchpad ）是分门别类整理过的，这不可容忍，今天必须给它收拾了。

安装应用后，如果一个应用的新版本有了新图标，启动台可能会同时显示旧图标和新图标。（即使是 应用名 和 bundleID 都一样）。

我首先启动了图标相同的两个应用，看了一下两个应用名、版本号和 bundleID 都是一样。

这个时候很可能是 启动台（Launchpad）的缓存造成的，那么我们可以使用下面的方法来试一试。

## 1、重启 Mac

重启大法永远是最好的、最有效的、最直接暴力的。

如果正在使用中，不想重启 Mac，还有一个方法。

## 2、重启 Launchpad

Launchpad 依赖于 Dock 应用，可以在终端使用下面的命令来重启 Dock 和启动台：

```
killall Dock
```

## 3、删除 Dock 数据

如果两个重启大法都不好使，那就只能删除 Dock 的数据，然后重启 Dock。

删除 Dock 的数据后，你对 Dock 的设置什么的就没有，所以尽可能不使用这个方法。

### 3.1、文件夹

使用放达前往 `~/Library/Application Support/Dock/` 下，删除`desktoppicture.db`（如果你有需要可以备份）。

![image-20200808020634923](https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20200808020634923.png)

然后重启 Launchpad 即可。

### 3.2、命令行

```
rm ~/Library/Application\ Support/Dock/*.db
killall Dock
```


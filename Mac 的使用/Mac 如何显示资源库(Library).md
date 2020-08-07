# Mac 如何显示资源库(Library)

Mac 10.15.6 中资源库 Library 是默认隐藏的，要想找到此文件夹有下面两种方法：

## 1、临时前往

打开放达的菜单栏上的前往，然后按住 option 键，在前往菜单中就会出现资源库(Library)选项。

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20200808021243957.png" alt="image-20200808021243957" style="zoom: 50%;" />

## 2、永久显示

在用户文件夹下，按住快捷键 Command + j ，或者如下图操作：

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20200808022149207.png" alt="image-20200808022149207" style="zoom:50%;" />

开启<显示“资源库”文件夹>，然后就可以在终端驶入下面命令，来控制资源库(Library)的显示和隐藏了：

```
 chflags nohidden ~/Library/
 chflags hidden ~/Library
```


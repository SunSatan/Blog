# Homebrew 安装

### 检查

首先打开终端，查看电脑是否安装 Homebrew。

```
 brew -v
```

出现类似以下的 Homebrew 版本信息，说明已经安装好了。

```
Homebrew 2.5.6-78-g914483b-dirty
Homebrew/homebrew-core (git revision 08590d; last commit 2020-10-19)
Homebrew/homebrew-cask (git revision e3151; last commit 2020-10-19)
```

如果没有则会出现 not found。

```
-bash brew: commnd not found
```

### 安装 Homebrew

[Homebrew 中网官网](https://brew.sh/index_zh-cn)

打开终端，输入以下命令：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

这里推荐使用国内镜像：

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

然后根据指示步骤安装即可。

安装完毕返回开头检查是否安装成功。
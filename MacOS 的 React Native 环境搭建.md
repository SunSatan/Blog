# MacOS 的 React Native 环境搭建

MacOS 的 React Native 的环境搭建需要使用 **Homebrew** 来安装 **npm**、 **Node.js** 、 **WatchMan** 、 **Flow** 、 **yarn**。

### 安装 npm 和 Node.js

npm 是随同 Node.js 一起安装的包管理工具，主要是解决 Node.js代码 部署上的问题。

npm 和 Node.js 是捆绑安装的，终端输入以下命令，之后就都不用管了。

```
brew install node
```

安装 npm 和 Node.js 后，可以设置 npm 镜像以及科学上网，来加速后面的安装过程。

```
npm config set registry https://registry.npm.taobao.org --global

npm config set disturl https://npm.taobao.org/dist --global
```

### 安装 yarn 和 React Native

Yarn 是由 Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具，用来取代 npm，可以加速 node 模块的下载。

```
npm install -g yarn react-native-cli
```

安装之后，同样可以设置 yarn 的镜像。

```
yarn config set registry https://registry.npm.taobao.org --global

yarn config set disturl https://npm.taobao.org/dist --global
```

### 安装 WatchMan

WatchMan 是由 Facebook 提供的监视文件系统变更的工具，并且可以触发指定的操作，可以提高开发时的性能（packager 可以快速捕捉文件的变化从而实现实时刷新）。

```
brew install watchman
```

### 安装Flow

flow是一个 JavaScript 的静态类型检查器，建议安装，以方便找出代码中可能存在的类型错误。

```
brew install flow
```

### 创建第一个 React Native 项目

终端输入：

```
react-native init ReactNativeProject
```

至此环境搭建完成。
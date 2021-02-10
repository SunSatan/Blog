# LLVM 编译 #

1、获取源码

```sh
git clone --depth 1 https://github.com/llvm/llvm-project.git
```

2、配置和构建 LLVM 和 Clang

==MacOS 10.15系统以上 默认的 Shell 是 zsh ，10.15 系统以下使用 bash、下边的 zshrc 改为 bashrc 即可。==

```sh
//向~/.zshrc文件导入 OSX_COMMANDLINE_SDKROOT变量（指定为 "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"）
echo 'export OSX_COMMANDLINE_SDKROOT="/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"' >> ~/.zshrc 

//使更改后的文件内容生效
source ~/.zshrc	
```

如果之前删掉了 CommandLineTools，那么在 `/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk` 路径将不存在，在编译阶段会打印如下错误：

```sh
error: unable to read SDK settings for '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk'
```



Mac OS 11：

```sh
cmake -G Xcode -j 4 -DLLVM_ENABLE_PROJECTS='libcxx;libc++;clang;lldb' -DLLDB_USE_SYSTEM_DEBUGSERVER=ON -DLLDB_TEST_COMPILER=clang++ -DCMAKE_OSX_SYSROOT="/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk" ../llvm
```

Mac OS 10.15：

```sh
cmake -G Xcode -j 4 -DLLVM_ENABLE_PROJECTS='libcxx;libc++;clang;lldb' -DLLDB_USE_SYSTEM_DEBUGSERVER=ON -DLLDB_TEST_COMPILER=clang++ -DCMAKE_OSX_SYSROOT="/Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk" ../llvm
```

<img src="https://cdn.jsdelivr.net/gh/SunSatan/PictureBed@master/uPic/image-20210116202644363.png" alt="image-20210116202644363" style="zoom:67%;" />
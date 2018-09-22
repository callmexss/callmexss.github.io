---
title: vckpg 使用小记
date: 2017-12-14 16:06:57
categories: tools
tags: 
  - vs 
  - package control
---

## vcpkg介绍

先把官方仓库的连接撂在[这里](https://github.com/Microsoft/vcpkg) 。

### 一个来自官方仓库的概述

> Vcpkg helps you get C and C++ libraries on Windows. This tool and ecosystem are currently in a preview state; your involvement is vital to its success.
>
> For short description of available commands, run `vcpkg help`.

总而言之，vcpkg可以看做Windows下C和C++的包管理工具，使用它可以方便的安装和使用**常用**的C、C++库。

### 快速开始

系统要求：

- Windows 10, 8.1, or 7
- Visual Studio 2017 or Visual Studio 2015 Update 3
- Git
- *Optional: CMake 3.10.0*  // 可选

clone项目到本地，执行以下操作：

```sh
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
# run bootstrap-vcpkg.bat
.\bootstrap-vcpkg.bat
# Then, to hook up user-wide integration, run (note: requires admin on first use)
.\vcpkg integrate install
# 直接 vcpkg.exe integrate install 也可以，上面应该是在power shell中使用的命令
# 这个命令就是使该工具的作用范围为全局用户范围
# Install any packages with
.\vcpkg install package1 package2 ...
.\vcpkg integrate install 
```

然后打开 Visual Studio 2017 或者 2015，直接使用`#include`导入头文件，愉快的使用吧~

### 配置Tab自动补全

vcpkg支持自动补全，包括**命令、包名、选项**等 ，通过输入以下命令在power shell中开启自动补全功能：

```sh
.\vcpkg integrate powershell
```

重启power shell即可。

### 更多信息

查看托管在*ReadTheDocs*的[文档](https://vcpkg.readthedocs.io/) 。

## vcpkg安装gtest

先使用`search`命令搜索一下gtest是否包含在支持安装的列表中：

```sh
C:\vcpkg>vcpkg.exe search gtest
gtest                1.8-1            GoogleTest and GoogleMock testing framewor
ks.

If your library is not listed, please open an issue at and/or consider making a
pull request:
    https://github.com/Microsoft/vcpkg/issues
```

使用`install`命令进行安装:

```sh
C:\vcpkg>vcpkg.exe install gtest
The following packages will be built and installed:
    gtest:x86-windows
Starting package 1/1: gtest:x86-windows
Building package gtest:x86-windows...
-- CURRENT_INSTALLED_DIR=C:/vcpkg/installed/x86-windows
-- DOWNLOADS=C:/vcpkg/downloads
-- CURRENT_PACKAGES_DIR=C:/vcpkg/packages/gtest_x86-windows
-- CURRENT_BUILDTREES_DIR=C:/vcpkg/buildtrees/gtest
-- CURRENT_PORT_DIR=C:/vcpkg/ports/gtest/.
-- Downloading https://github.com/google/googletest/archive/release-1.8.0.tar.gz
...
-- Downloading https://github.com/google/googletest/archive/release-1.8.0.tar.gz
... OK
-- Testing integrity of downloaded file...
-- Testing integrity of downloaded file... OK
-- Extracting source C:/vcpkg/downloads/google-googletest-release-1.8.0.tar.gz
-- Extracting done
-- Applying patch C:/vcpkg/ports/gtest/0001-Enable-C-11-features-for-VS2015-fix-
appveyor-fail.patch
-- Applying patch C:/vcpkg/ports/gtest/0001-Enable-C-11-features-for-VS2015-fix-
appveyor-fail.patch done
-- Configuring x86-windows-rel
-- Configuring x86-windows-rel done
-- Configuring x86-windows-dbg
-- Configuring x86-windows-dbg done
-- Build x86-windows-rel
-- Build x86-windows-rel done
-- Build x86-windows-dbg
-- Build x86-windows-dbg done
-- Installing: C:/vcpkg/packages/gtest_x86-windows/share/gtest/copyright
-- Performing post-build validation
-- Performing post-build validation done
Building package gtest:x86-windows... done
Installing package gtest:x86-windows...
Installing package gtest:x86-windows... done
Elapsed time for package gtest:x86-windows: 3.147 min

Total elapsed time: 3.147 min

The package gtest is compatible with built-in CMake targets:

    enable_testing()
    find_package(GTest REQUIRED)
    target_link_libraries(main PRIVATE GTest::GTest GTest::Main)
    add_test(AllTestsInMain main)
```

使用`list`命令查看x86 windows桌面版下gtest是否安装成功：

```sh
C:\vcpkg>vcpkg.exe list
gtest[core]:x86-windows        1.8-1            GoogleTest and GoogleMock testin
g frameworks.
```

还可以通过在包名后面指定架构安装特定的包，例如：

```sh
# x86-uwp:  Universal Windows Platform
# x64-windows:  x64 Desktop
C:\vcpkg>vcpkg.exe install sqlite3:x86-uwp zlib:x64-windows 

# 查看所有支持的目标
C:\vcpkg>vcpkg.exe help triplet 
Available architecture triplets:
  arm-uwp
  arm64-uwp
  x64-uwp
  x64-windows-static
  x64-windows
  x86-uwp
  x86-windows-static
  x86-windows
```

最后，使用`integrate`命令使得gtest可以被用于所有当前用户的项目：

```sh
C:\vcpkg>vcpkg.exe integrate install
Applied user-wide integration for this vcpkg root.

All MSBuild C++ projects can now #include any installed libraries.
Linking will be handled automatically.
Installing new libraries will make them instantly available.

CMake projects should use: "-DCMAKE_TOOLCHAIN_FILE=C:/vcpkg/scripts/buildsystems
/vcpkg.cmake"
# 使用 vcpkg.exe integrate remove 移除整合
```

 至此，一次`vcpkg`的使用就结束了，比起之前自己编译配置路径确实方便了不少，关键还几乎不会出错，是很赞了。


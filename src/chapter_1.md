# Chapter 1

# Cairo库安装

在本章中，我们将学习如何在计算机上安装Cairo库和相关依赖项。

## 操作系统支持

Cairo可在Linux、macOS和Windows等操作系统上运行。我们将分别讨论这些操作系统上的安装方法。

## Linux

在Linux上，您可以使用包管理器来安装Cairo。

### Debian/Ubuntu

如果您正在使用Debian或Ubuntu，可以使用以下命令来安装Cairo：

```
sudo apt-get install libcairo2-dev
```

### Fedora/RHEL/CentOS

如果您正在使用Fedora、RHEL或CentOS，可以使用以下命令来安装Cairo：

```
sudo dnf install cairo-devel
```

## macOS

在macOS上，使用Homebrew工具可以轻松安装Cairo库。

首先，打开终端并运行以下命令来安装Homebrew：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

然后，运行以下命令来安装Cairo：

```
brew install cairo
```

## Windows

在Windows上，您可以下载预编译的二进制文件并手动安装Cairo。

请注意，在Windows操作系统上安装Cairo需要安装MSYS2和MinGW-w64，因此请按照以下步骤进行安装：

1. 下载并安装MSYS2。您可以从[官方网站](https://www.msys2.org/)下载安装程序。

2. 打开MSYS2终端并运行以下命令：

   ```
   pacman -S mingw-w64-x86_64-cairo
   ```

3. 安装完成后，您现在可以使用Cairo库了。

## 结论

通过本章，我们学习了如何在Linux、macOS和Windows上安装Cairo库。这是开始使用Cairo进行编程的第一步。

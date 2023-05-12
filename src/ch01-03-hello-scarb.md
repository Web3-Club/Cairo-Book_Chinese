# Hello, Scarb!
# Scarb 包管理器

Scarb 是开罗语的包管理器，受到 Rust 构建系统和包管理器 Cargo 的启发。它可以处理诸如构建代码、下载所需库以及构建这些库等多个任务。

## 安装

### 要求

在安装之前，需要确保 Git 可执行文件已经被添加到 PATH 环境变量中。

### 安装步骤

目前，Scarb 需要手动安装，以下是安装步骤：

1. 从 [Scarb releases on GitHub](https://github.com/swmansion/scarb/releases) 下载与您的操作系统和 CPU 架构相匹配的发布存档。
2. 将其提取到您希望安装 Scarb 的位置，例如 `~/scarb`。
3. 将 Scarb 的二进制可执行文件路径添加到 PATH 环境变量中。具体方法根据您使用的 shell 不同而不同。以 zsh 为例，假设您将 Scarb 提取到了 `~/scarb`，则在 `~/.zshrc` 文件末尾添加以下行：`export PATH="$PATH:~/scarb/bin"`
4. 在新终端会话中运行以下命令来验证安装：`scarb --version`，它应该会打印出 Scarb 和 Cairo 语言版本信息。

## 使用 Scarb 创建项目

让我们使用 Scarb 创建一个新项目，并查看它与最初的 "Hello, world!" 项目有何不同。

首先进入您的项目目录（或者您想存储代码的任何位置），然后执行以下命令：

```bash
scarb new hello_scarb
```

这将在当前目录下创建一个名为 `hello_scarb` 的新项目。运行此命令后，会生成两个文件和一个目录：一个 Scarb.toml 文件、包含 lib.cairo 文件的 src 目录，以及一个 .gitignore 文件。同时，它还会初始化一个新的 Git 存储库。

打开 Scarb.toml 文件，您将看到 TOML 格式的配置信息，用于设置项目的名称和版本号，并列出任何依赖项。对于我们的 "Hello, world!" 项目，我们不需要任何其他依赖项，因此可以保留默认值。

在 src 目录中，存在一个空的 lib.cairo 文件。让我们清空其内容，并添加以下一行代码：

```cairo
mod hello_scarb;
```

稍后，我们将解释此代码的含义和用途。
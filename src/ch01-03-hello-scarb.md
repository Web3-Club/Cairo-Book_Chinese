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

我们可以创建一个名为src/hello_scarb.cairo的新文件，并将以下代码放入其中：

文件名：src/hello_scarb.cairo


```
use debug::PrintTrait;
fn main() {
    'Hello, Scarb!'.print();
}
```

我们刚刚创建了一个名为lib.cairo的文件，其中包含一个模块声明引用了另一个名为“hello_scarb”的模块，以及包含“hello_scarb”模块的实现细节的文件hello_scarb.cairo。

Scarb要求你的源代码文件位于src目录中。

顶层项目目录保留用于README文件、许可信息、配置文件和任何其他非代码相关内容。Scarb确保为所有项目组件指定位置，保持结构化组织。

如果你启动的项目不使用Scarb，就像我们使用“Hello, world！”项目一样，你可以将其转换为使用Scarb的项目。将项目代码移动到src目录中并创建一个合适的Scarb.toml文件即可。

## 构建Scarb项目
从你的hello_scarb目录中，输入以下命令构建你的项目：

```
$ scarb build
   Compiling hello_scarb v0.1.0 (file:///projects/Scarb.toml)
    Finished release target(s) in 0 seconds
```

此命令在target/release中创建一个sierra文件，现在我们先忽略sierra文件。

如果你正确安装了Cairo，你应该能够运行并看到以下输出：

```
$ cairo-run src/lib.cairo
[DEBUG] Hello, Scarb!                   (raw: 5735816763073854913753904210465)
```

运行成功，返回[]。
注意：你会注意到这里我们没有使用Scarb命令，而是直接使用了Cairo二进制文件中的命令。由于Scarb尚未具有执行Cairo代码的命令，因此我们必须直接使用cairo-run命令。在本教程的其余部分中，我们将使用该命令，但我们也将使用Scarb命令来初始化项目。

## 定义自定义脚本
我们可以在Scarb.toml文件中定义Scarb脚本，用于执行自定义shell脚本。将以下行添加到Scarb.toml文件中：

```
[scripts]
run-lib = "cairo-run src/lib.cairo"
```

现在你可以运行以下命令来运行该项目：

```
$ scarb run run-lib
[DEBUG] Hello, Scarb!                   (raw: 5735816763073854913753904210465)
```

运行成功，返回[]。

使用scarb run是一种方便的方式来运行自定义shell脚本，它可以帮助你运行文件并测试你的项目。


让我们总结一下我们已经学到的Scarab知识：

- 我们可以使用scarb new创建一个项目。
- 我们可以使用scarb build构建项目以生成编译后的Sierra代码。
- 我们可以在Scarb.toml中定义自定义脚本，并使用scarb run命令调用它们。
- 使用Scarb的另一个优点是，无论你在哪个操作系统上工作，命令都是相同的。因此，此时我们将不再为Linux和macOS与Windows提供特定的说明。

## 总结
你已经开始了Cairo之旅！在本章中，你学会了：

- 安装最新稳定版的Cairo。
- 使用cairo-run直接编写和运行“Hello, world！”程序。
- 使用Scarb惯例创建和运行一个新项目。

现在是一个建立更加实质性的程序来逐渐熟悉阅读和编写Cairo代码的好时机。
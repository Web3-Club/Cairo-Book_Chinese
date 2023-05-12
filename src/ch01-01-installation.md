## 安装

安装Cairo的第一步是从cairo存储库手动下载或使用安装脚本进行下载。您需要互联网连接以进行下载。

### 先决条件

首先，您需要安装Rust和Git。如果您没有安装它们，可以通过运行以下命令来安装它们：

```bash
# 安装稳定版Rust
rustup override set stable && rustup update
```

然后，安装Git。

### 使用脚本（由Fran编写的安装程序）安装Cairo

要使用Installer by Fran脚本安装Cairo，请运行以下命令：

```bash
curl -L https://github.com/franalgaba/cairo-installer/raw/main/bin/cairo-installer | bash
```

如果你想要安装特定版本的Cairo而不是最新版本，则设置 `CAIRO_GIT_TAG` 环境变量 (例如：`export CAIRO_GIT_TAG=v1.0.0-alpha.6`)。

安装Cairo后，请按照以下说明设置您的shell环境。

### 更新

要更新Cairo，请运行以下命令：

```bash
rm -fr ~/.cairo
curl -L https://github.com/franalgaba/cairo-installer/raw/main/bin/cairo-installer | bash
```

### 卸载

要卸载Cairo，请删除 `$CAIRO_ROOT` 目录，该目录通常为 `~/.cairo`。

从您的 `.bashrc` 文件中删除以下行：

```bash
export PATH="$HOME/.cairo/target/release:$PATH"
```

然后重启您的shell：

```bash
exec $SHELL
```

### 为Cairo设置您的Shell环境

将环境变量 `CAIRO_ROOT` 定义为指向Cairo存储其数据的路径。默认值为 `$HOME/.cairo`，但如果您通过Git checkout安装了Cairo，则建议将其设置为与克隆它的位置相同的位置。

如果尚未添加 `cairo-*` 可执行文件，请将它们添加到您的 `PATH` 中。按照下面的说明为您的shell进行设置。

#### 对于Bash

```bash
echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.bashrc
echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.bashrc
```

如果您有 `.profile`、`.bash_profile` 或 `.bash_login` 文件，请在其中添加上述命令。如果没有这些文件之一，请将它们添加到您的`.profile` 中。

```bash
echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.profile
echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.profile
```

#### 对于Zsh

```bash
echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.zshrc
echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.zshrc
```

如果您希望非交互式登录shell中使用Cairo，请还将这些命令添加到您的 `.zprofile` 或 `.zlogin` 中。

#### 对于Fish

对于 Fish 3.2.0 或更新版本：

```bash
set -Ux CAIRO_ROOT $HOME/.cairo
fish_add_path $CAIRO_ROOT/target/release
```

否则：

```bash
set -Ux CAIRO_ROOT $HOME/.cairo
set -U fish_user_paths $CAIRO_ROOT/target/release $fish_user_paths
```

在MacOS上，您可能还想安装一个名为 Fig 的工具，它为许多命令行工具提供替代的shell补全功能，并在终端窗口中提供类似于IDE的弹出式界面。

### 手动安装Cairo（由Abdel编写的指南）

步骤1：安装Cairo 1.0

如果您使用的是x86 Linux系统并且可以使用发布二进制文件，请从此处下载Cairo：https://github.com/starkware-libs/cairo/releases。

以下是安装Cairo的指南：

## 步骤1：从源代码编译Cairo

首先，您需要定义环境变量`CAIRO_ROOT`，可以通过以下命令进行设置：

``` bash
export CAIRO_ROOT="${HOME}/.cairo"
```

如果该文件夹不存在，则需创建`.cairo`文件夹：

``` bash
mkdir $CAIRO_ROOT
```

然后将Cairo编译器克隆到`$CAIRO_ROOT`目录下（默认根目录）：

``` bash
cd $CAIRO_ROOT && git clone git@github.com:starkware-libs/cairo.git .
```

如果您想要安装特定版本的编译器，可以使用以下命令查看可用的版本：

``` bash
git fetch --all --tags
git describe --tags `git rev-list --tags`
```

然后，使用`git checkout`命令检出所需版本。例如，对于v1.0.0-alpha.6版本，可以执行以下命令：

``` bash
git checkout tags/v1.0.0-alpha.6
```

最后，生成发布二进制文件：

``` bash
cargo build --all --release
```

## 注意事项：保持Cairo更新

由于您已经在本地克隆了Cairo编译器的存储库，所以只需使用以下命令即可拉取最新更改并重新构建：

``` bash
cd $CAIRO_ROOT && git fetch && git pull && cargo build --all --release
```

## 步骤2：将Cairo 1.0可执行文件添加到PATH

使用以下命令将Cairo 1.0的可执行文件添加到`$PATH`环境变量中：

``` bash
export PATH="$CAIRO_ROOT/target/release:$PATH"
```

注意：如果您使用Linux二进制文件进行安装，则需相应地调整目标路径。

## 步骤3：设置语言服务器

### VS Code扩展程序

在进行以下步骤之前，请禁用先前的Cairo 0.x扩展程序。然后按照以下步骤安装Cairo 1扩展程序以获得正确的语法高亮和代码导航：

1. 打开VS Code并转到菜单“View”，然后选择“Extensions”；
2. 在搜索栏中输入“Cairo”；
3. 选择“Cairo Language Support”；
4. 点击“Install”按钮。

### Cairo语言服务器

在完成步骤1后，您将已经构建了`cairo-language-server`二进制文件。执行以下命令将其路径复制到剪贴板中（注意：以下命令适用于macOS系统）：

``` bash
which cairo-language-server | pbcopy
```

然后，将该路径粘贴到Cairo 1.0扩展程序的`languageServerPath`设置中。
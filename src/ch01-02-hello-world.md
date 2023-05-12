# Hello, World!

现在你已经安装了Cairo，是时候编写你的第一个Cairo程序了。在学习一门新语言时，往往会写一个简单的程序，在屏幕上输出“Hello, world!”这句话，因此我们也将在这里完成相同的任务！

**注意：本书假定读者熟悉基本的命令行操作。Cairo对代码编辑或工具使用没有特别的要求，你可以使用自己喜欢的集成开发环境（IDE）代替命令行。Cairo团队已经为Cairo语言开发了一个VSCode插件，您可以使用它来获得来自语言服务器和代码高亮的功能。有关更多详细信息，请参见附录A。**

## 创建项目目录

首先，需要创建一个用于存储Cairo代码的目录。Cairo并不关心代码存放的位置，但是对于本书中的练习和项目，建议在用户主目录下创建一个`cairo_projects`目录，并在其中保存所有项目。

打开一个终端窗口，输入以下命令，以创建`cairo_projects`目录和`cairo_projects`目录中的“Hello, world！”项目目录：

- 对于Linux、macOS和PowerShell on Windows，请输入以下命令：

  ```
  mkdir ~/cairo_projects
  cd ~/cairo_projects
  mkdir hello_world
  cd hello_world
  ```

- 对于Windows CMD，请输入以下命令：

  ```
  > mkdir "%USERPROFILE%\projects"
  > cd /d "%USERPROFILE%\projects"
  > mkdir hello_world
  > cd hello_world
  ```

## 编写和运行Cairo程序

接下来，创建一个名为`main.cairo`的新源文件。Cairo文件总是以`.cairo`扩展名结尾。如果文件名中包含多个单词，则惯例是使用下划线来分隔它们。例如，使用`hello_world.cairo`而不是`helloworld.cairo`。

现在打开刚刚创建的`main.cairo`文件，并输入清单1-1中的代码。

**文件名：`main.cairo`**

```cairo
use debug::PrintTrait;
fn main() {
    'Hello, world!'.print();
}
```

**清单1-1：一个打印“Hello, world！”的程序。**

保存文件并回到`~/cairo_projects/hello_world`目录的终端窗口。输入以下命令来编译和运行文件：

```
$ cairo-run main.cairo
Hello, world！
```

无论您使用的是哪种操作系统，都应该在终端上输出字符串“Hello, world!”。

如果“Hello, world！”已经被打印出来，恭喜你！你已经正式编写了一个Cairo程序。这意味着你成为了一个Cairo程序员——欢迎加入！

## Cairo程序的解剖

让我们详细查看这个“Hello, world！”程序。这是谜题的第一部分：

```cairo
fn main() {

}
```

这些行定义了一个名为`main`的函数。`main`函数很特殊：它是每个可执行Cairo程序中第一个运行的代码。在这里，第一行声明了一个名为`main`的函数，它没有参数并且不返回任何值。如果有参数，它们将放在括号`() `中。

函数体包含在`{}`中。Cairo需要在所有函数体周围使用花括号。好的习惯是将左花括号与函数声明放在同一行，在它们之间添加一个空格。

**注意：如果您希望在不同的Cairo项目中使用标准样式，可以使用自动格式化工具`cairo-format`以特定的风格格式化代码（有关`cairo-format`的更多信息请参
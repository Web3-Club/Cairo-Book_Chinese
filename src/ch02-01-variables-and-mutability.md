# 变量和可变性

Cairo使用不可变的内存模型，这意味着一旦写入内存单元，就无法覆盖而只能从中读取。为了反映这种不可变的内存模型，Cairo中的变量默认是不可变的。但是，该语言抽象了这种模型，并为您提供了使变量可变的选项。让我们探讨一下Cairo如何强制实行不可变性，以及如何使变量可变。

当变量是不可变的时，一旦将一个值绑定到一个名称，就无法更改该值。为了说明这一点，在cairo_projects目录中生成一个名为variables的新项目，使用命令scarb new variables。

然后，在您的新variables目录中，打开src/lib.cairo文件并将其代码替换为以下代码，该代码目前无法编译：

        use debug::PrintTrait;
        fn main() {
            let x = 5;
            x.print();
            x = 6;
            x.print();
        }

保存并使用cairo-run src/lib.cairo运行程序。您应该会收到有关不可变性错误的错误消息，如以下输出所示：

        error: Cannot assign to an immutable variable.
         --> lib.cairo:5:5
            x = 6;
            ^***^

        Error: failed to compile: src/lib.cairo


该示例演示了编译器如何帮助您查找程序中的错误。编译器错误可能让人沮丧，但它们只意味着您的程序尚未安全地执行所需的操作；它们不意味着您不是一个好的程序员！经验丰富的Caironautes仍然会出现编译器错误。

您收到了错误消息 Cannot assign to an immutable variable. ，因为您尝试为不可变x变量分配第二个值。

当我们试图更改被指定为不可变的值时，获得编译时错误非常重要，因为这种情况可能导致错误。如果我们的代码的一部分在假设一个值永远不会改变，而我们的代码的另一部分更改了该值，那么这种错误的原因可能在事后很难追踪，特别是当第二段代码只有有时更改该值时。 Cairo编译器保证了当您声明一个值不会改变时，它确实不会改变，因此您不必自己跟踪它。因此，您的代码更容易推理。

但是，可变性非常有用，可以使代码更方便。尽管变量默认情况下是不可变的，但是可以在变量名称前添加mut来使它们可变。添加mut还通过指示代码的未来读者将更改该变量的值来传达意图。

然而，此时您可能会想知道当一个变量被声明为mut时到底发生了什么，因为我们之前提到Cairo的内存是不可变的。答案是，Cairo的内存是不可变的，但是变量指向的内存地址可以更改。在检查低级Cairo汇编代码时，变量突变被实现为语法糖，它将变异操作转换为等效于变量遮蔽的一系列步骤。唯一的区别是在Cairo级别，变量没有被重新声明，因此它的类型不能改变。

例如，让我们将src/lib.cairo更改为以下内容：

        use debug::PrintTrait;
        fn main() {
            let mut x = 5;
            x.print();
            x = 6;
            x.print();
        }

现在运行该程序，我们会得到以下输出：
        [DEBUG]                                (raw: 5)
        
        [DEBUG]                                (raw: 6)
        
        Run completed successfully, returning []

当使用mut时，我们被允许将x绑定的值从5更改为6。最终，决定是否使用可变性取决于您，这取决于您认为在特定情况下最清晰的内容。

常量

与不可变变量一样，常量是将值绑定到名称并且不允许更改的值，但常量和变量之间有一些区别。

首先，您不允许使用mut来定义常量。常量默认情况下是不可变的 - 它们始终是不可变的。您可以使用const关键字而不是let关键字来声明常量，值的类型必须用注释标注。我们将在下一节“数据类型”中介绍类型和类型注释，所以现在不必担心细节。只需知道您必须始终注释类型。
常量只能在全局范围内声明，这使它们对于许多代码部分需要了解的值非常有用。
最后一个区别是常量只能设置为常量表达式，而不能设置为仅在运行时才能计算的值的结果。当前仅支持字面量常量。
以下是常量声明的示例：

    const ONE_HOUR_IN_SECONDS: u32 = 3600;

Cairo对常量的命名惯例是使用所有大写字母，并在单词之间使用下划线。
常量对于在程序运行的整个时间内都有效，在声明它们的作用域内是非常有用的。这个属性使得常量对于应用程序域中的值对于程序的多个部分可能需要了解的情况非常有用，比如一种游戏中任何玩家被允许获得的最大分数，或者光速的速度等等。
将硬编码值用作常量在传达该值的含义方面对于未来代码维护者非常有用。如果硬编码值需要在未来更新，则只需要更改代码中的一个地方即可。
变量屏蔽
变量屏蔽是指使用与先前变量相同的名称声明新变量。 Caironautes说第一个变量被第二个变量遮盖，这意味着当您使用变量名时，编译器将看到第二个变量。实际上，第二个变量超越第一个变量，使变量名称的任何使用为自己，直到它自己被屏蔽或作用域结束。我们可以使用相同变量的名称并重复使用let关键字来遮罩变量，如下所示：
文件名：src/lib.cairo

    use debug::PrintTrait;
    fn main() {
        let x = 5;
        let x = x + 1;
        {
            let x = x * 2;
            'Inner scope x value is:'.print();
            x.print()
        }
        'Outer scope x value is:'.print();
        x.print();
    }

此程序首先将x绑定到值5，然后通过重复 let x = 创建新变量x，取原始值并将其加1，以便x的值为6。然后，在用花括号创建的内部范围内，第三个let语句也遮盖了x并创建了一个新变量，将以前的值乘以2，以使x的值为12。当该范围结束时，内部遮盖将结束，x将返回到6。当我们运行这个程序时，它会输出以下内容：

    cairo-run src/lib.cairo
    [DEBUG] Inner scope x value is:         (raw: 7033328135641142205392067879065573688897582790068499258)

    [DEBUG]
                                            (raw: 12)

    [DEBUG] Outer scope x value is:         (raw: 7610641743409771490723378239576163509623951327599620922)

    [DEBUG]                                 (raw: 6)

运行成功完成，返回[]
变量屏蔽与将变量标记为mut不同，因为如果意外尝试重新分配给此变量而不使用let关键字，我们将获得编译时错误。使用let，我们可以对值执行一些转换，但在完成这些转换后，变量必须是不可变的。
mut和遮蔽之间的另一个区别是，当我们再次使用let关键字时，我们实际上正在创建一个新变量，这允许我们改变值的类型，同时重用相同的名称。如前所述，变量屏蔽和可变变量在较低级别上是等效的。唯一的区别是通过变量屏蔽，如果您更改其类型，编译器不会抱怨变量。例如，假设我们的程序在u64和felt252类型之间执行类型转换。

        let x = 2;
        x.print();
        let x: felt252 = x.into(); // converts x to a felt, type annotation is required.
        x.print()


第一个 x 变量具有 u64 类型，而第二个变量 x 具有 felt252 类型。因此，使用变量屏蔽可以避免我们必须想出不同的名称，例如 x_u64 和 x_felt252；相反，我们可以重用更简单的 x 名称。然而，如果我们尝试在此处使用 mut，则会在编译时出现错误：

        use debug::PrintTrait;
        use traits::Into;
        fn main() {
            let mut x = 2;
            x.print();
            x = x.into();
            x.print()
        }
        

错误信息表明我们期望取得一个 u64（原始类型），但我们获得了一个不同的类型：

        error: Unexpected argument type. Expected: "core::integer::u64", found: "core::felt252".
         --> lib.cairo:6:9
            x = x.into();
                ^******^
        Error: failed to compile: src/lib.cairo


现在，我们已经探讨了变量的工作方式，让我们来看一下它们可以具有的更多数据类型。

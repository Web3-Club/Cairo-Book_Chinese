# 控制流

控制流是大多数编程语言中的基本构建块，它允许根据条件运行一些代码，并在某个条件为真时重复运行一些代码。让您控制Cairo代码执行的最常见结构是if表达式和循环。

## if表达式

if表达式允许根据条件分支代码。您提供一个条件，然后声明：“如果满足此条件，请运行此代码块。如果条件不成立，请不要运行此代码块。”

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    let number = 3;

    if number == 5 {
        'condition was true'.print();
    } else {
        'condition was false'.print();
    }
}
```

所有if表达式都以关键字if开头，后跟条件。在这种情况下，条件检查变量number是否具有等于5的值。我们将要执行的代码块放在条件后面的花括号内，用于执行条件为true的条件块。

可选地，我们还可以包含else表达式，我们选择在这里使用它，以便程序有替代代码块可执行，如果条件评估为false。如果您不提供else表达式并且条件为false，则程序将跳过if块并继续执行下一段代码。

尝试运行此代码; 您应该会看到以下输出：

```
$ cairo-run main.cairo
[DEBUG]	condition was false
```

让我们尝试更改number的值，以使条件为真，看看会发生什么：

```cairo
let number = 5;
$ cairo-run main.cairo
condition was true
```

还值得注意的是，此代码中的条件必须是bool类型。如果条件不是bool，则会出现错误。

```cairo
$ cairo-run main.cairo
thread 'main' panicked at 'Failed to specialize: `enum_match<felt252>`. Error: Could not specialize libfunc `enum_match` with generic_args: [Type(ConcreteTypeId { id: 1, debug_name: None })]. Error: Provided generic argument is unsupported.', crates/cairo-lang-sierra-generator/src/utils.rs:256:9
```

## 使用else if处理多个条件

您可以通过将if和else组合在else if表达式中来使用多个条件。例如：

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    let number = 3;

    if number == 12 {
        'number is 12'.print();
    } else if number == 3 {
        'number is 3'.print();
    } else if number - 2 == 1 {
        'number minus 2 is 1'.print();
    } else {
        'number not found'.print();
    }
}
```

这个程序有四个可能的路径。运行它后，您应该会看到以下输出：

```
[DEBUG]	number is 3
```

当此程序执行时，它依次检查每个if表达式，并执行第一个条件为真的主体。请注意，即使number - 2 == 1为真，我们也看不到输出number minus 2 is 1' .print()，也没有从else块中看到not found文本。这是因为Cairo仅执行第一个真条件的块，并且一旦它找到一个条件，它甚至不会检查其余部分。使用太多的else if表达式可能会使您的代码混乱，因此如果有多个，您可能需要重构代码。第5章描述了用于这些情况的强大的Cairo分支结构，称为match。

## 在let语句中使用if

因为if是一个表达式，我们可以在let语句的右侧使用它，将结果赋给一个变量。

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    let condition = true;
    let number = if condition {
        5
    } else {
        10
    };

    number.print();
}
```

在这个例子中，我们声明了一个变量condition并将其设置为true。然后我们定义了一个新变量number，并使用if表达式初始化它。如果条件为真，则将5分配给number。否则，将10分配给number。

最终，我们调用print()方法来打印变量的值。尝试运行此程序，您应该会看到输出：

```
[DEBUG]	5
```

## 循环

循环结构允许您多次执行一种或多种操作。Cairo支持while和for循环。

### while循环

while循环不断地重复执行代码，直到条件为false为止。例如，以下代码片段计算从1到10的数字的总和：

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    let mut sum = 0;
    let mut i = 1;

    while i <= 10 {
        sum += i;
        i += 1;
    }

    sum.print();
}
```

在这个例子中，我们定义了两个变量：sum和i。我们使用while循环来重复执行代码块，每次将i添加到sum中，然后递增i的值。当i等于11时，while循环的条件评估为false，并退出循环。

您可以使用类似的方式编写无限循环：只需编写while true或while 1就可以了。

### for循环

for循环是一种更常见的循环结构，它按照一定的步骤重复执行代码。例如，以下代码片段打印从1到10的数字：

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    for i in 1..11 {
        i.print();
    }
}
```

在这个例子中，我们使用for循环将i从1逐步增加到10，并在每次迭代时打印它的值。请注意，1..11是一个范围对象，表示从1到10的整数（包括1，但不包括11）。您可以使用类似的方式声明其他类型的范围：例如，'a'..'z'将为字母a到z创建一个范围。

### break和continue语句

Cairo还支持break和continue语句，它们可以在循环内部控制执行流程。

break语句用于立即停止当前循环并退出该循环。例如，以下代码片段计算从1开始的第一个平方和大于100的数字：

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    let mut sum = 0;
    let mut i = 1;

    loop {
        let square = i * i;

        if sum + square > 100 {
            break;
        }

        sum += square;
        i += 1;
    }

    sum.print();
}
```

在这个例子中，我们使用loop语句来创建一个无限循环。在每次迭代中，我们计算当前数字的平方并检查是否将其添加到sum中将使得sum超过100。如果是这样，我们使用break语句停止循环。否则，我们将数字的平方添加到sum中，并递增i的值。

continue语句用于立即跳过当前迭代并开始下一次迭代。例如，以下代码片段打印从1到10之间的偶数：

文件名：main.cairo

```cairo
use debug::PrintTrait;

fn main() {
    for i in 1..11 {
        if i % 2 == 1 {
            continue;
        }

        i.print();
    }
}
```

在这个例子中，我们在for循环内部使用if表达式来检查i是否为奇数。如果是这样，我们使用continue语句跳过该迭代。否则，我们打印数字的值。因此，我们只会看到输出2、4、6、8和10。

## 总结

控制流是编程中非常重要的概念，它允许您根据条件运行一些代码，并在某些情况下重复运行一些代码。Cairo支持if表达式、while循环和for循环。您还可以使用break和continue语句来控制循环内部的执行流程。熟练掌握这些结构对于编写高效和可读性良好的代码至关重要。

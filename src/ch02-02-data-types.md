# 数据类型

Cairo中的每个值都是某种数据类型，这告诉Cairo正在指定哪种数据，以便它知道如何处理该数据。这一节涵盖了两个数据类型的子集：标量和复合数据类型。

请记住，Cairo是一种静态类型语言，这意味着在编译时必须确定所有变量的类型。编译器通常可以根据值及其用法推断出所需的类型。在许多类型可能的情况下，我们可以使用一个转换方法，其中指定所需的输出类型。例如，下面代码使用TryInto和OptionTrait trait将felt252类型转换为u32类型：

use traits::TryInto;
use option::OptionTrait;
fn main() {
    let x: felt252 = 3;
    let y: u32 = x.try_into().unwrap();
}


您将看到其他数据类型的不同类型注释。

标量数据类型

标量类型表示单个值。Cairo有三种基本标量类型：felts、整数和布尔值。您可能会从其他编程语言中认识到它们。让我们看看它们在Cairo中的工作原理。

Felt类型

在Cairo中，如果您没有指定变量或参数的类型，它的类型默认为一个位元素，表示为关键字felt252。在Cairo的上下文中，当我们说“一个位元素”时，我们意思是一个在范围0 <= x < P内的整数，其中P是一个非常大的素数，当前等于P = 2^{251} + 17 * 2^{192}+1。当执行加法、减法或乘法时，如果结果超出了指定范围的素数范围，将发生溢出，并添加或减去适当的P倍数，以将结果带回范围内（即，结果计算为模P）。

整数和字段元素之间最重要的区别是除法：字段元素的除法（因此在Cairo中的除法）与常规CPU的除法不同，其中整数除法x / y被定义为[x/y]，其中返回商的整数部分（因此您获得7/3=2），并且它可能或可能不满足方程（x / y）*y == x，取决于x是否可被y整除。在Cairo中，x/y的结果被定义为始终满足方程（x / y）*y == x。如果y被作为整数划分x，您将在Cairo中得到预期结果（例如6/2确实会产生3）。但是当y不能整除x时，您可能会得到意想不到的结果。例如，由于2 * ((P+1)/2) = P+1 ≡ 1 mod[P]，在Cairo中，1/2的值为(P+1)/2（而不是0或0.5），因为它满足上述方程式。

整数类型

felt252类型是一种基本类型，用作创建核心库中所有类型的基础。然而，强烈建议程序员只要可能就使用整数类型而不是felt252类型，因为整数类型带有附加的安全功能，可以提供额外的保护以防止代码中潜在的漏洞，例如溢出检查。通过使用这些整数类型，程序员可以确保他们的程序更加安全，并且不太容易受到攻击或其他安全威胁的影响。整数是没有分数部分的数字。此类型声明指示程序员可以用来存储整数的比特数。表3-1显示了Cairo中的内置整数类型。我们可以关于如何选择使用哪种类型的整数？尝试估计您的整数可以具有的最大值并选择合适的大小。使用usize的主要情况是在索引某种集合时。

数字运算

Cairo支持您预期的所有整数类型的基本数学运算：加法，减法，乘法，除法和余数运算。整数除法向最接近的整数向零截断。以下代码显示了您将如何在let语句中使用每个数值操作：

fn main() {
    // addition
    let sum = 5_u128 + 10_u128;

    // subtraction
    let difference = 95_u128 - 4_u128;

    // multiplication
    let product = 4_u128 * 30_u128;

    // division
    let quotient = 56_u128 / 32_u128; // result is 1
    let quotient = 64_u128 / 32_u128; // result is 2

    // remainder
    let remainder = 43_u128 % 5_u128; // result is 3
}

这些语句中的每个表达式都使用数学运算符并评估为单个值，然后将其绑定到变量中。付录B包含Cairo提供的所有运算符的列表。

布尔类型

与大多数其他编程语言一样，Cairo中的布尔类型具有两个可能的值：true和false。布尔值的大小为一个felt252（注意和Python等其他语言可能不同）。Cairo中的布尔类型是使用bool指定的。例如：

fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}


使用布尔值的主要方法是通过条件语句，例如if表达式。我们将在“控制流”部分中介绍if表达式的工作方式。

短字符串类型

Cairo没有原生的字符串类型，但可以将形成我们称之为“短字符串”的字符存储在felt252中。短字符串最大长度为31个字符。这是为了确保它可以适合单个felt中（一个felt为252位，一个ASCII字符为8位）。以下是在单引号之间放置的声明值的一些示例：

    let my_first_char = 'C';
    let my_first_string = 'Hello world';


类型转换

在Cairo中，可以使用TryInto和Into特征提供的try_into和into方法将标量类型从一种类型转换为另一种类型。在目标类型可能不适合源值的情况下，try_into方法允许安全类型转换。请记住，try_into返回Option <T>类型，您需要解包它以访问新值。另一方面，当成功保证时，例如当源类型小于目标类型时，可以使用into方法进行类型转换。要执行转换，请在源值上调用var.into（）或var.try_into（），以将其转换为另一种类型。必须明确定义新变量的类型，如下例所示：

`
use traits::TryInto;
use traits::Into;
use option::OptionTrait;

fn main() {
    let my_felt252 = 10;
    // Since a felt252 might not fit in a u8, we need to unwrap the Option<T> type
    let my_u8: u8 = my_felt252.try_into().unwrap();
    let my_u16: u16 = my_u8.into();
    let my_u32: u32 = my_u16.into();
   let my_u64: u64 = my_u32.into();
    let my_u128: u128 = my_u64.into();
    // As a felt252 is smaller than a u256, we can use the into() method
    let my_u256: u256 = my_felt252.into();
    let my_usize: usize = my_felt252.try_into().要从元组中获取单个值，我们可以使用模式匹配来解构元组值，就像这样：

use debug::PrintTrait;
fn main() {
    let tup = (500, 6, true);

    let (x, y, z) = tup;

    if y == 6 {
        'y is six!'.print();
    }
}


该程序首先创建一个元组并将其绑定到变量tup。然后使用带有let的模式将tup变成三个单独的变量x、y和z。这称为解构，因为它将单个元组分成三个部分。最后，程序打印y is six，因为y的值为6。

我们还可以同时声明元组的值和类型。例如：
fn main() {
    let (x, y): (i32, i32) = (2, 3);
}

单元类型()

单元类型是仅有一个值()的类型。它表示一个没有元素的元组。它的大小始终为零，并且保证不会存在于编译后的代码中。

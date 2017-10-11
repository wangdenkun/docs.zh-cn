---
title: "Lambda 表达式"
description: "了解如何使用 Lambda 表达式，它们是可作为参数传递的可执行代码块。"
keywords: ".NET, .NET Core, lambda 表达式, lambda, 委托"
ms-author: ronpet
author: rpetrusha
ms.date: 11/22/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: b6a0539a-8ce5-4da7-adcf-44be345a2714
ms.translationtype: HT
ms.sourcegitcommit: 2762cdc983465979a530192716c33de7044dd1ed
ms.openlocfilehash: 659a3366b00d6abe6598c31774d008c6b8f400fd
ms.contentlocale: zh-cn
ms.lasthandoff: 08/04/2017

---

# <a name="lambda-expressions"></a>Lambda 表达式 #

Lambda 表达式是作为对象处理的代码块（表达式或语句块）。 它可作为参数传递给方法，也可通过方法调用返回。 Lambda 表达式广泛用于：

- 将要执行的代码传递给异步方法，例如 @System.Threading.Tasks.Task.Run (System.Action)。

- 编写 [LINQ 查询表达式](linq/index.md)。

- 创建[表达式树](expression-trees-building.md)。

Lambda 表达式是可以表示为委托的代码，或者表示为表达式树的代码，它所表示的表达式树可以编译为委托。 Lambda 表达式的特定委托类型取决于其参数和返回值。 不返回值的 Lambda 表达式对应于 `Action` 委托，具体取决于其参数数量。 返回值的 Lambda 表达式对应于 `Func` 委托，具体取决于其参数数量。 例如，有 2 个参数但不返回值的 Lambda 表达式对应于 @System.Action%602 委托。 有 1 个参数并返回值的 Lambda 表达式对应于 @System.Func%602 委托。

Lambda 表达式使用 [lambda 声明运算符](language-reference/operators/lambda-operator.md) `=>` 从其可执行代码中分离 lambda 参数列表。 若要创建 Lambda 表达式，需要在 lambda 运算符左侧指定输入参数（如果有），然后在另一侧输入表达式或语句块。 例如，单行 Lambda 表达式 `x => x * x` 指定名为 `x` 的参数并返回 `x` 的平方值。 如下面的示例所示，你可以将此表达式分配给委托类型：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/lambda1.cs#1)]

或者，可以将其直接作为方法参数传递：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/lambda2.cs#2)]

## <a name="expression-lambdas"></a>表达式 lambda ##

 表达式位于 => 运算符右侧的 Lambda 表达式称为“表达式 lambda”。 表达式 lambda 广泛用于[表达式树](expression-trees.md)的构造。 表达式 lambda 会返回表达式的结果，并采用以下基本形式：

```csharp
(input parameters) => expression
```

仅当 lambda 只有一个输入参数时，括号才是可选的；否则括号是必需的。 使用空括号指定零个输入参数：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/expression3.cs#1)]

括号内的两个或更多输入参数使用逗号加以分隔：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/expression3.cs#2)]

通常，编译器在确定参数类型时使用类型推理。 但是，编译器有时难以或无法推断输入类型。 如果出现这种情况，可显式指定类型，如以下示例所示：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/expression3.cs#3)]

在上一个示例中，请注意表达式 Lambda 的主体可以包含一个方法调用。 但是，如果要创建在 .NET Framework 外部（例如 SQL Server 或实体框架 (EF)）计算的表达式树，则应避免在 Lambda 表达式中使用方法调用，因为这些方法在 .NET 实现的上下文之外可能没有任何意义。 在这种情况下，如果选择使用方法调用，请务必对其进行全面测试，确保可成功解析此方法调用。

## <a name="statement-lambdas"></a>语句 lambda ##

语句 lambda 与表达式 lambda 表达式类似，只是语句括在大括号中：

```csharp
(input parameters) => { statement; }
```

语句 lambda 的主体可以包含任意数量的语句；但是，实际上通常不会多于两个或三个。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/statement1.cs#1)]

像匿名方法一样，语句 lambda 也不能用于创建表达式目录树。

## <a name="async-lambdas"></a>异步 lambda ##

通过使用 [async](language-reference/keywords/async.md) 和 [await](language-reference/keywords/await.md) 关键字，可以轻松创建包含异步处理的 Lambda 表达式和语句。 例如，本示例调用异步执行的 `ShowSquares` 方法。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/async1.cs#1)]

有关如何创建和使用异步方法的详细信息，请参阅[使用 Async 和 Await 的异步编程](programming-guide/concepts/async/index.md)。

## <a name="lambda-expressions-and-tuples"></a>Lambda 表达式和元组 ##

从 C# 7.0 开始，C# 语言为元组提供内置支持。 可以提供一个元组作为 Lambda 表达式的参数，同时 Lambda 表达式也可以返回元组。 在某些情况下，C# 编译器使用类型推理来确定元组组件的类型。 

可通过用括号括住用逗号分隔的组件列表来定义元组。 以下示例使用包含 5 个组件的元组将一系列数字传递给 Lambda 表达式，此 Lambda 表达式将每个值加倍，然后返回包含乘法运算结果的 5 个组件的元组。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/tuples1.cs#1)]

通常，元组字段命名为 `Item1`、`Item2` 等等。但是，可以使用命名组件定义元组，如以下示例所示。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/tuples2.cs#1)]

有关对 C# 中元组的支持的详细信息，请参阅 [C# 元组类型](tuples.md)。

## <a name="lambdas-with-the-standard-query-operators"></a>含标准查询运算符的 lambda ##

在其他实现中，LINQ to Objects 有一个输入参数，其类型是泛型委托 @System.Func%601 系列中的一种。 这些委托使用类型参数来定义输入参数的数量和类型，以及委托的返回类型。 `Func` 委托对于封装用户定义的表达式非常有用，这些表达式将应用于一组源数据中的每个元素。 例如 @System.Func%601 委托，其语法为：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#1)]

可使用如下代码实例化委托

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#2)]

其中，`int` 是一个输入参数，`bool` 是返回值。 返回值始终在最后一个类型参数中指定。 调用以下 `Func` 委托时，该委托将返回 true 或 false 以指示输入参数是否等于 5：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#3)]

参数类型为 @System.Linq.Expressions.Expression%601 时，也可以提供 Lambda 表达式，例如在 @System.Linq.Queryable 类型内定义的标准查询运算符中提供。 指定 @System.Linq.Expressions.Expression%601 参数时，lambda 编译为表达式树。 以下示例使用 [System.Linq.Enumerable.Count](xref:System.Linq.Enumerable.Count%60%601(System.Collections.Generic.IEnumerable{%60%600})) 标准查询运算符。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#4)]

编译器可以推断输入参数的类型，或者你也可以显式指定该类型。 这个特殊 Lambda 表达式可计算那些除以 2 时余数为 1 的整数的数量 (`n`)。

以下示例生成一个序列，其中包含 `numbers` 数组中 位于 9 之前的所有元素，因为这是序列中第一个不满足条件的数字。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#5)]

以下示例通过将输入参数括在括号中来指定多个输入参数。 该方法返回数字数组中的所有元素，直至遇到一个值小于数组中其序号位置的数字为止。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/query1.cs#6)]

## <a name="type-inference-in-lambda-expressions"></a>Lambda 表达式中的类型推理 ##

编写 lambda 时，通常不必为输入参数指定类型，因为编译器可以根据 lambda 主体、参数类型以及 C# 语言规范中描述的其他因素来推断类型。 对于大多数标准查询运算符，第一个输入是源序列中的元素类型。 如果要查询 `IEnumerable<Customer>`，则输入变量将被推断为 `Customer` 对象，这意味着你可以访问其方法和属性：

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/infer1.cs#1)]

Lambda 类型推理的一般规则如下：

- Lambda 包含的参数数量必须与委托类型包含的参数数量相同。

- Lambda 中的每个输入参数必须都能够隐式转换为其对应的委托参数。

- Lambda 的返回值（如果有）必须能够隐式转换为委托的返回类型。

请注意，lambda 表达式本身没有类型，因为常规类型系统没有“Lambda 表达式”这一内部概念。 但是，有时以一种非正式的方式谈论 lambda 表达式的“类型”会很方便。 在这些情况下，类型是指委托类型或 lambda 表达式所转换到的 @System.Linq.Expressions.Expression 类型。

## <a name="variable-scope-in-lambda-expressions"></a>Lambda 表达式中的变量范围 ##

在定义 lambda 函数的方法内或包含 Lambda 表达式的类型内，Lambda 可以引用范围内的外部变量（请参阅[匿名方法](programming-guide/statements-expressions-operators/anonymous-methods.md)）。 以这种方式捕获的变量将进行存储以备在 lambda 表达式中使用，即使在其他情况下，这些变量将超出范围并进行垃圾回收。 必须明确地分配外部变量，然后才能在 lambda 表达式中使用该变量。 以下示例演示了这些规则。

[!code-csharp[csSnippets.Lambdas](../../samples/snippets/csharp/concepts/lambda-expressions/scope.cs#1)]

 下列规则适用于 lambda 表达式中的变量范围：

- 捕获的变量将不会被作为垃圾回收，直至引用变量的委托符合垃圾回收的条件。

- 在外部方法中看不到 lambda 表达式内引入的变量。

- Lambda 表达式无法从封闭方法中直接捕获 `ref` 或 `out` 参数。

- Lambda 表达式中的返回语句不会导致封闭方法返回。

- 如果跳转语句的目标在块外部，则 lambda 表达式不能包含位于 lambda 函数内部的 `goto` 语句、 `break` 语句或 `continue` 语句。 同样，如果目标在块内部，则在 lambda 函数块外部使用跳转语句也是错误的。

## <a name="see-also"></a>请参阅 ##

[LINQ（语言集成查询）](../standard/using-linq.md)   
[匿名方法](programming-guide/statements-expressions-operators/anonymous-methods.md)   
[表达式树](expression-trees.md)


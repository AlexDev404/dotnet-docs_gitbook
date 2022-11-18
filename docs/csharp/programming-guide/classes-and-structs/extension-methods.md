---
title: "Extension Methods - C# Programming Guide"
description: Extension methods in C# enable you to add methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type.
ms.date: 03/19/2020
helpviewer_keywords: 
  - "methods [C#], adding to existing types"
  - "extension methods [C#]"
  - "methods [C#], extension"
ms.assetid: 175ce3ff-9bbf-4e64-8421-faeb81a0bb51
---

# Extension Methods (C# Programming Guide)

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are static methods, but they're called as if they were instance methods on the extended type. For client code written in C#, F# and Visual Basic, there's no apparent difference between calling an extension method and the methods defined in a type.

The most common extension methods are the LINQ standard query operators that add query functionality to the existing <xref:System.Collections.IEnumerable?displayProperty=nameWithType> and <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> types. To use the standard query operators, first bring them into scope with a `using System.Linq` directive. Then any type that implements <xref:System.Collections.Generic.IEnumerable%601> appears to have instance methods such as <xref:System.Linq.Enumerable.GroupBy%2A>, <xref:System.Linq.Enumerable.OrderBy%2A>, <xref:System.Linq.Enumerable.Average%2A>, and so on. You can see these additional methods in IntelliSense statement completion when you type "dot" after an instance of an <xref:System.Collections.Generic.IEnumerable%601> type such as <xref:System.Collections.Generic.List%601> or <xref:System.Array>.

### OrderBy Example

The following example shows how to call the standard query operator `OrderBy` method on an array of integers. The expression in parentheses is a lambda expression. Many standard query operators take lambda expressions as parameters, but this isn't a requirement for extension methods. For more information, see [Lambda Expressions](../../language-reference/operators/lambda-expressions.md).

[!code-csharp[csProgGuideExtensionMethods#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#3)]

Extension methods are defined as static methods but are called by using instance method syntax. Their first parameter specifies which type the method operates on. The parameter is preceded by the [this](../../language-reference/keywords/this.md) modifier. Extension methods are only in scope when you explicitly import the namespace into your source code with a `using` directive.

The following example shows an extension method defined for the <xref:System.String?displayProperty=nameWithType> class. It's defined inside a non-nested, non-generic static class:

[!code-csharp[csProgGuideExtensionMethods#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#4)]

The `WordCount` extension method can be brought into scope with this `using` directive:

```csharp
using ExtensionMethods;
```

And it can be called from an application by using this syntax:

```csharp
string s = "Hello Extension Methods";
int i = s.WordCount();
```

You invoke the extension method in your code with instance method syntax. The intermediate language (IL) generated by the compiler translates your code into a call on the static method. The principle of encapsulation is not really being violated. Extension methods cannot access private variables in the type they are extending.

Both the `MyExtensions` class and the `WordCount` method are `static`, and it can be accessed like all other `static` members. The `WordCount` method can be invoked like other `static` methods as follows:

```csharp
string s = "Hello Extension Methods";
int i = MyExtensions.WordCount(s);
```

The preceding C# code:

- Declares and assigns a new `string` named `s` with a value of `"Hello Extension Methods"`.
- Calls `MyExtensions.WordCount` given argument `s`

For more information, see [How to implement and call a custom  extension method](./how-to-implement-and-call-a-custom-extension-method.md).

In general, you'll probably be calling extension methods far more often than implementing your own. Because extension methods are called by using instance method syntax, no special knowledge is required to use them from client code. To enable extension methods for a particular type, just add a `using` directive for the namespace in which the methods are defined. For example, to use the standard query operators, add this `using` directive to your code:

```csharp
using System.Linq;
```

(You may also have to add a reference to System.Core.dll.) You'll notice that the standard query operators now appear in IntelliSense as additional methods available for most <xref:System.Collections.Generic.IEnumerable%601> types.

## Binding Extension Methods at Compile Time

You can use extension methods to extend a class or interface, but not to override them. An extension method with the same name and signature as an interface or class method will never be called. At compile time, extension methods always have lower priority than instance methods defined in the type itself. In other words, if a type has a method named `Process(int i)`, and you have an extension method with the same signature, the compiler will always bind to the instance method. When the compiler encounters a method invocation, it first looks for a match in the type's instance methods. If no match is found, it will search for any extension methods that are defined for the type, and bind to the first extension method that it finds. The following example demonstrates how the compiler determines which extension method or instance method to bind to.

## Example

The following example demonstrates the rules that the C# compiler follows in determining whether to bind a method call to an instance method on the type, or to an extension method. The static class `Extensions` contains extension methods defined for any type that implements `IMyInterface`. Classes `A`, `B`, and `C` all implement the interface.

The `MethodB` extension method is never called because its name and signature exactly match methods already implemented by the classes.

When the compiler can't find an instance method with a matching signature, it will bind to a matching extension method if one exists.

[!code-csharp[csProgGuideExtensionMethods#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#5)]

## Common Usage Patterns

### Collection Functionality

In the past, it was common to create "Collection Classes" that implemented the <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> interface for a given type and contained functionality that acted on collections of that type. While there's nothing wrong with creating this type of collection object, the same functionality can be achieved by using an extension on the <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>. Extensions have the advantage of allowing the functionality to be called from any collection such as an <xref:System.Array?displayProperty=nameWithType> or <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> that implements <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> on that type. An example of this using an Array of Int32 can be found [earlier in this article](#orderby-example).

### Layer-Specific Functionality

When using an Onion Architecture or other layered application design, it's common to have a set of Domain Entities or Data Transfer Objects that can be used to communicate across application boundaries. These objects generally contain no functionality, or only minimal functionality that applies to all layers of the application. Extension methods can be used to add functionality that is specific to each application layer without loading the object down with methods not needed or wanted in other layers.

```csharp
public class DomainEntity
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

static class DomainEntityExtensions
{
    static string FullName(this DomainEntity value)
        => $"{value.FirstName} {value.LastName}";
}
```

### Extending Predefined Types

Rather than creating new objects when reusable functionality needs to be created, we can often extend an existing type, such as a .NET or CLR type. As an example, if we don't use extension methods, we might create an `Engine` or `Query` class to do the work of executing a query on a SQL Server that may be called from multiple places in our code. However we can instead extend the <xref:System.Data.SqlClient.SqlConnection?displayProperty=nameWithType> class using extension methods to perform that query from anywhere we have a connection to a SQL Server. Other examples might be to add common functionality to the <xref:System.String?displayProperty=nameWithType> class, extend the data processing capabilities of the <xref:System.IO.File?displayProperty=nameWithType> and <xref:System.IO.Stream?displayProperty=nameWithType> objects, and <xref:System.Exception?displayProperty=nameWithType> objects for specific error handling functionality. These types of use-cases are limited only by your imagination and good sense.

Extending predefined types can be difficult with `struct` types because they're passed by value to methods. That means any changes to the struct are made to a copy of the struct. Those changes aren't visible once the extension method exits. You can add the `ref` modifier to the first argument of an extension method. Adding the `ref` modifier means the first argument is passed by reference. This enables you to write extension methods that change the state of the struct being extended.

## General Guidelines

While it's still considered preferable to add functionality by modifying an object's code or deriving a new type whenever it's reasonable and possible to do so, extension methods have become a crucial option for creating reusable functionality throughout the .NET ecosystem. For those occasions when the original source isn't under your control, when a derived object is inappropriate or impossible, or when the functionality shouldn't be exposed beyond its applicable scope, Extension methods are an excellent choice.

For more information on derived types, see [Inheritance](../../fundamentals/object-oriented/inheritance.md).

When using an extension method to extend a type whose source code you aren't in control of, you run the risk that a change in the implementation of the type will cause your extension method to break.

If you do implement extension methods for a given type, remember the following points:

- An extension method will never be called if it has the same signature as a method defined in the type.
- Extension methods are brought into scope at the namespace level. For example, if you have multiple static classes that contain extension methods in a single namespace named `Extensions`, they'll all be brought into scope by the `using Extensions;` directive.

For a class library that you implemented, you shouldn't use extension methods to avoid incrementing the version number of an assembly. If you want to add significant functionality to a library for which you own the source code, follow the .NET guidelines for assembly versioning. For more information, see [Assembly Versioning](../../../standard/assembly/versioning.md).

## See also

- [C# Programming Guide](../index.md)
- [Parallel Programming Samples (these include many example extension methods)](/samples/browse/?products=dotnet-core%2Cdotnet-standard&term=parallel)
- [Lambda Expressions](../../language-reference/operators/lambda-expressions.md)
- [Standard Query Operators Overview](../concepts/linq/standard-query-operators-overview.md)
- [Conversion rules for Instance parameters and their impact](/archive/blogs/sreekarc/conversion-rules-for-instance-parameters-and-their-impact)
- [Extension methods Interoperability between languages](/archive/blogs/sreekarc/extension-methods-interoperability-between-languages)
- [Extension methods and Curried Delegates](/archive/blogs/sreekarc/extension-methods-and-curried-delegates)
- [Extension method Binding and Error reporting](/archive/blogs/sreekarc/extension-method-binding-and-error-reporting)
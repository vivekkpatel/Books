```markdown
## C# in Depth by Jon Skeet

```
## Chapter 1 : Survival of the sharpest

### An evolving language

C# has been a statically typed language from the start: your code specifies the types of variables, parameters, values returned from methods, and so on. **The more precisely you can specify the shape of the data your code accepts and returns, the more the compiler can help you avoid mistakes.**

**That’s particularly true as the application you’re building grows.** If you can see all the code for your whole program on one screen (or at least hold it all in your head at one time), a statically typed language doesn’t have much benefit. As the scale increases, it becomes increasingly important that your code concisely and effectively communicates what it does. You can do that through documentation, but static typing lets you communicate in a machine-readable way.

As C# has evolved, its type system has allowed more fine-grained descriptions. The most obvious example of this is **generics**. In C# 1, you might have had code like this:

```csharp
public class Bookshelf
{
 public IEnumerable Books { get { ... } }
}
```
**What type is each item in the Books sequence? The type system doesn’t tell you. With generics in C# 2, you can communicate more effectively:**

```csharp
public class Bookshelf
{
 public IEnumerable<Book> Books { get { ... } }
}
```
C# 2 also brought **nullable value** types, thereby **allowing the absence of information to be expressed effectively without resorting to magic values** such as –1 for a collection index or DateTime.MinValue for a date.

The plans for C# 8 include **nullable reference** types, which will allow even more communication. Up to this point, nothing in the language lets you express whether a reference (either as a return value, a parameter, or just a local variable) might be null. This leads to error-prone code if you’re not careful and boilerplate validation code if you are careful, neither of which is ideal. C# 8 will expect that anything not explicitly nullable is intended not to be nullable. For example, consider a method declaration like this:

```csharp
string Method(string x, string? y)
```

The parameter types indicate that the argument corresponding to x shouldn’t be null but that the argument corresponding to y may be null. The return type indicates that the method won’t return null.

### Ever more concise code

One of the recurring themes within new features of C# has been the ability to let you express your ideas in ways that are increasingly concise. The type system is part of this, as you’ve seen with anonymous types, but many other features also contribute to this. There are lots of words you might hear for this, especially in terms of what can be removed with the new features in place. C#’s features allow you to reduce ceremony, remove boilerplate code, and avoid cruft. These are just different ways of talking about the same effect. It’s not that any of the now-redundant code was wrong; it was just distracting and unnecessary.

## Chapter 2 : C# 2

### Generics

**Generics allow you to write general-purpose code that’s type safe at compile time using the same type in multiple places without knowing what that type is beforehand.** When generics were first introduced, their primary use was for collections, but in modern C# code, they crop up everywhere.

### Generics save the day

List<T> solves all the problems

- You don’t need to know the size of the collection beforehand, unlike with arrays.
- The exposed API uses T everywhere it needs to refer to the element type, so you know that a List will contain only string references. You’ll get a compile-time error if you try to add anything else, unlike with `ArrayList`.
- You can use it with any element type without worrying about generating code and managing the result, unlike with `StringCollection` and similar types.

### Type constraints

When a type parameter is declared by a generic type or method, it can also specify type constraints that restrict which types can be provided as type arguments.

```jsx
static void PrintItems<T>(List<T> items)!
```

### Nullable value types

#### Aim: Expressing an absence of information

Sometimes it’s useful to have a variable to **represent some information, but that information won’t be present in every situation**. Here are a few simple examples:

- You’re modeling a customer order, including the company’s details, but the customer may not be ordering on behalf of a company.
- You’re modeling a person, including their date of birth and date of death, but the person may still be alive.
- You’re modeling a filter for products, including a price range, but the customer may not have specified a maximum price.

### CLR and framework support: The Nullable<T> struct

The core of nullable value type support is the Nullable<T> struct.

```csharp
Nullable<int> nullable = new Nullable<int>();
Console.WriteLine(nullable.HasValue);
```

## Chapter 3 : LINQ and everything that comes with it

### Automatically implemented properties

Prior to C# 3, every property had to be implemented manually with bodies for the get and/or set accessors. The compiler was happy to provide an implementation for field-like events but not properties. That meant there were a lot of properties like this:

```csharp
private string name;
public string Name
{
 get { return name; }
 set { name = value; }
}
```

Formatting would vary by code style, but whether the property was one long line, 11 short ones, or five lines in between (as in the preceding example), it was always just noise. It was a very long-winded way of expressing the intention to have a field and expose its value to callers via a property. C# 3 made this much simpler by using automatically implemented properties (often referred to as automatic properties or even autoprops). These are properties with no accessor bodies; the compiler provides the implementation. The whole of the preceding code can be replaced with a single line:

```csharp
public string Name { get; set; }
```

### Implicit typing

#### Typing terminology

Many terms are used to describe the way programming languages interact with their type system. Some people use the terms weakly typed and strongly typed, but I try to avoid those because they’re not clearly defined and mean different things to different developers. Two other aspects have more consensus: **static/dynamic typing and explicit/implicit typing.**

### STATIC AND DYNAMIC TYPING

Languages that are statically typed are typically compiled languages; the compiler is able to determine the type of each expression and check that it’s used correctly. For example, if you make a method call on an object, the compiler can use the type information to check that there’s a suitable method to call based on the type of the expression the method is called on, the name of the method, and the number and types of the arguments. Determining the meaning of something like a method call or field access is called binding. Languages that are dynamically typed leave all or most of the binding to execution time.

### Chapter 4 : Improving interoperability

### Dynamic Typing

#### Usage suggestions

I’ll be up front about this: I’m generally not a fan of dynamic typing. I can’t remember the last time I used it in production code, and I’d do so only warily and after a lot of testing for correctness and performance.

I’m a sucker for static typing. In my experience, it gives four significant benefits:

- When I make mistakes, I’m likely to discover them earlier—at compile time rather than execution time. That’s particularly important with code paths that may be hard to test exhaustively.
- Editors can provide code completion. This isn’t particularly important in terms of speed of typing, but it’s great as a way of exploring what I might want to do next, particularly if I’m using a type I’m unfamiliar with. Editors for dynamic languages can provide remarkable code-completion facilities these days, but they’ll never be quite as precise as those for statically typed languages, because there just isn’t as much information available.
- It makes me think about the API I’m providing, in terms of parameters, return types, and so on. After I’ve made decisions about which types to accept and return, that acts as ready-made documentation: I need to add comments only for anything that isn’t otherwise obvious, such as the range of acceptable values.
- By doing work at compile time instead of execution time, statically typed code usually has performance benefits over dynamically typed code. I don’t want to emphasize this too much, as modern runtimes can do amazing things, but it’s certainly worth considering.

### Chapter 5 : Writing asynchronous code

Asynchrony has been a thorn in the side of developers for years. It’s been known to be useful as a way of avoiding tying up a thread while waiting for some arbitrary task to complete, but it’s also been a pain in the neck to implement correctly

Even within the .NET Framework (which is still relatively young in the grand scheme of things), we’ve had three models to try to make things simpler:

- The BeginFoo/EndFoo approach from .NET 1.x, using IAsyncResult and AsyncCallback to propagate results
- The event-based asynchronous pattern from .NET 2.0, as implemented by BackgroundWorker and WebClient
- The Task Parallel Library (TPL) introduced in .NET 4.0 and expanded in .NET 4.5

## Chapter 7 : c#5 Bonus Features

### Caller information attributes

#### Basic behavior

.NET 4.5 introduced three new attributes:

- CallerFilePathAttribute
- CallerLineNumberAttribute
- CallerMemberNameAttribute

These are all in the System.Runtime.CompilerServices namespace. Just as with other attributes, when you apply any of these, you can omit the Attribute suffix. Because that’s the most common way of using attributes, I’ll abbreviate the names appropriately for the rest of the book.

## Chapter 8 : Super-sleek properties and expression-bodied members

### Upgrades to automatically implemented properties

#### Read-only automatically implemented properties

C# 6 allows genuinely read-only properties backed by read-only fields to be expressed in a simple way. All it takes is an empty getter and no setter, as shown in the next listing.

```csharp
public sealed class Point
{
 public double X { get; }
 public double Y { get; }
	public Point(double x, double y)
	{
 	 X = x;
 	 Y = y;
	}
}
```

## Chapter 9 : Stringy features

### A recap on string formatting in .NET

#### Simple string formatting

```csharp
Console.Write("What's your name? ");
string name = Console.ReadLine();
Console.WriteLine("Hello, {0}!", name);
```

## Chapter 10 : A smörgåsbord of features for concise code

### Using static directives

#### Importing static members

The following listing shows the complete method with the uses of System.Math in bold. I’ve omitted the class declaration for convenience. It could be in a CoordinateConverter class, or it could be a factory method in the Point type itself.

```csharp
using System;
...
static Point PolarToCartesian(double degrees, double magnitude)
{
 double radians = degrees * Math.PI / 180;
 return new Point(
 Math.Cos(radians) * magnitude,
 Math.Sin(radians) * magnitude);
}
```

## Chapter 11 : Composition using tuples

### Introduction to tuples

Tuples allow you to create a single composite value from multiple individual values. They’re shorthand for composition with no extra encapsulation for situations where values are related to each other but you don’t want the work of creating a new type.

## Chapter 12 : Deconstruction and Pattern Matching

### Deconstruction of tuples

```csharp
var tuple = (10, "text");
var (a, b) = tuple;
(int c, string d) = tuple;
int e;
string f;
(e, f) = tuple;
Console.WriteLine($"a: {a}; b: {b}");
Console.WriteLine($"c: {c}; d: {d}");
Console.WriteLine($"e: {e}; f: {f}");
```

## Chapter 13 : Improving efficiency with more pass by reference

### Ref local and Ref return

#### NO REFERENCES TO READ-ONLY VARIABLES

```csharp
class MixedVariables
{
 private int writableField;
 private readonly int readonlyField;
 public void TryIncrementBoth()
 {
 ref int x = ref writableField;
 ref int y = ref readonlyField;
 x++;
 y++;
 }
}
```

### Ref returns

```csharp
static void Main()
{
 int x = 10;
 ref int y = ref RefReturn(ref x);
 y++;
 Console.WriteLine(x);
}
static ref int RefReturn(ref int p)
{
 return ref p;
}
```

## Chapter 14 : Concise code in C# 7

### Local methods

```csharp
static void Main()
{
 int x = 10;
 PrintAndIncrementX();
 PrintAndIncrementX();
 Console.WriteLine($"After calls, x = {x}");
 void PrintAndIncrementX()
 {
 Console.WriteLine($"x = {x}");
 x++;
 }
}
```

## Chapter 15 : C# 8 and beyond

### Nullable reference types

Ah, null references. The so-called billion-dollar mistake that Tony Hoare apologized for in 2009 after introducing them in the 1960s. It’s hard to find an experienced C# developer who hasn’t been bitten by a NullReferenceException at least a few times. The C# team has a plan to tame null references, making it clearer where we should expect to find them.

### What problem do nullable reference types solve?

```csharp
public class Customer
{
 public string Name { get; set; }
 public Address Address { get; set; }
}
public class Address
{
 public string Country { get; set; }
}
```

### Changing the meaning when using reference types

The broad idea of the null safety feature is to assume that when a developer is intentionally discriminating between non-null and nullable reference types, the default is to be non-nullable. New syntax is introduced for nullable reference types: string is a non-nullable reference type, and string? is a nullable reference type.

```csharp
public class Customer
{
 public string Name { get; set; }
 public Address? Address { get; set; }
 public Customer(string name) =>
 Name = name;
}
```

The compiler then uses that information to warn you if try to misuse a value that might be null.

### Indexes and ranges

```csharp
string quotedText = "'This text was in quotes'";
Console.WriteLine(quotedText);
Console.WriteLine(quotedText.Substring(1..^1));//Range

//output
'This text was in quotes'
This text was in quotes
```

# C# in Depth by Jon Skeet

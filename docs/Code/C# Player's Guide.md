---
title: The C# Player's Guide 
---
# The C# Player's Guide by RB Whitaker

## Hello World your first program

!!! quote
    *"You'll spend more time reading code than writing it. Do yourself a favor and go out of your way to make code easy to understand, regardless of what the compiler will tolerate."*  

## The C# Type System

### Integer types
- 1 byte = 8 bits
- so 1 byte can represent 2^8 = 256 different values
- u= unsigned, s= signed. 
- For most type u prefixes indicate that the type can only represent non-negative values. 
- The byte type is bit exception as it is unsigned by default. the byte type have signed counterpart sbyte.

| Type   | Size (bytes) | Min                      | Max                      |
|--------|--------------|--------------------------|--------------------------|
| byte   | 1            | 0                        | 255                      |
| sbyte  | 1            | -128                     | 127                      |
| short  | 2            | -32,768                  | 32,767                   |
| ushort | 2            | 0                        | 65,535                   |
| int    | 4            | -2,147,483,648           | 2,147,483,647            |
| uint   | 4            | 0                        | 4,294,967,295            |
| long   | 8            | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| ulong  | 8            | 0                        | 18,446,744,073,709,551,615 |

!!! tip
    On modern computer using byte instead of int for small numbers may actually be slower because of the way the CPU handles memory. If you use byte to store value then the JIT compiler often loads bytes into registers as 32-bit values anyway.  so when you use byte, short, or long instead of int, the CPU has to do extra work to convert the value to a size it can handle efficiently. Using byte only makes sense when you have a large array like storing pixel colors for 1920x1080 image. So best practice is to use int unless you have a specific reason to use a different type.

### The Digit Separator
- For larger numbers, you can use the digit separator to make them more readable.
- Since we can't use commas in numeric literals, we can use the underscore character (_) instead.

```csharp
int mobileNumber = 555_123_4567;
int worldPopulation = 7_900_000_000;
```

### Text Types
- C# has two primary text types: char and string.
- The char type represents a single Unicode character and is enclosed in single quotes (' ').
- The string type represents a sequence of characters and is enclosed in double quotes (" ").
- You can use Unicode escape sequences to represent characters that may not be easily typed on a keyboard.

```csharp
char letterA = 'A';
char aLetter = '\u0061';
char baseball = '⚾'; 
string greeting = "Hello, World!";
```

### Floating-Point Types

| Type   | Bytes | Min            | Max           | Decimals |
|--------|-------|----------------|---------------|----------|
| float  | 4     | ±1.0 x 10^-45  | ±3.4 x 10^38  | 7        |
| double | 8     | ±5.0 x 10^-324 | ±1.7 x 10^308 | 15       |
| decimal| 16    | ±1.0 x 10^-28  | ±7.9 x 10^28  | 28       |

```csharp
double number1 = 3.5623; 
// Scientific notation by employing 'e' to represent powers of 10
double avogadrosNumber = 6.022e23; 
float number2 = 3.5623f; 
decimal number3 = 3.5623m; 
```

!!! tip
    Similar to integer types, use double as default floating-point type unless you have a specific reason to use float or decimal. Moder JIT compiler optimizes double operations well on modern CPUs. Use float when working with large arrays of floating-point numbers to save memory, such as in graphics programming. Use decimal for financial calculations where precision is crucial to avoid rounding errors.

### Boolean Type
- The bool type represents a value that can be either true or false.
  

```csharp
bool isGameOver = false;
```

!!! info
    The bool type gets its name from the mathematician George Boole, who developed Boolean algebra, a branch of mathematics that deals with true and false values. 

## Math

### Prefix and Postfix Operators

```csharp
int x;
x = 5;
int y= ++x;
Console.WriteLine($"x={x}, y={y}");
// Output: x=6, y=6

x= 5;
int z= x++;
Console.WriteLine($"x={x}, z={z}");
// Output: x=6, z=5
```

### Type Casting
- Implicit Casting (automatically) - converting a smaller type to a larger type size
- Explicit Casting (manually) - converting a larger type to a smaller size type

## Arrays

### Indexing from the end
- You can use the ^ operator to index from the end of an array.

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };
int lastNumber = numbers[^1]; // 50
int secondLastNumber = numbers[^2]; // 40
```

### Ranges
- You can use the .. operator to create ranges in arrays.

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };
int[] subArray = numbers[1..4]; // { 20, 30, 40 }
int[] fromStart = numbers[..3]; // { 10, 20, 30
int[] toEnd = numbers[2..]; // { 30, 40, 50 }
int[] allNumbers = numbers[..]; // { 10, 20, 30, 40, 50 }
int[] lastTwo = numbers[^2..]; // { 40, 50 }
int[] middleNumbers = numbers[1..^1]; // { 20, 30, 40 }
```

## Null References

### Null Conditinal Operator
- The null-conditional operator (?.) allows you to safely access members of an object that might be null.

```csharp
string? nullableString = null;
int? length = nullableString?.Length; // length will be null
Console.WriteLine(length); // Output: 
```

```csharp
private string? GetTopPlayerName() 
{ 
return _scoreManager?.GetScores()?[0]?.Name; 
// Returns null if _scoreManager or GetScores() is null
} 
```

### The Null-Coalescing Operator
- The null-coalescing operator (??) allows you to provide a default value when a nullable type is null.

```csharp
string? userInput = null;
string displayText = userInput ?? "Default Value";
Console.WriteLine(displayText); // Output: Default Value
```

### The Null Forgiving Operator
- The null-forgiving operator (!) tells the compiler that you are sure a nullable reference type is not null at that point in the code.
- **Use this operator with caution, as it can lead to runtime exceptions if the value is actually null.**

```csharp
string? nullableString = "Hello, World!";
int length = nullableString!.Length; // Tells the compiler that nullableString is not null
Console.WriteLine(length); // Output: 13
```

## Records

### What is a Record?
- A record is a reference type that provides built-in functionality for encapsulating data.
- Records are immutable by default, meaning their properties cannot be changed after creation.
- Records provide value-based equality, meaning two record instances with the same data are considered equal.

### Creating a Record
- You can define a record using the `record` keyword.
- Here is an example of creating a simple record to represent a person:
- Single line of this code gives you
  - Constructor
  - Name and Age properties
  - Equals() value-based equality method
  - GetHashCode() method
  - Deconstruct() method

```csharp
public record person(string Name, int Age);

var person1 = new person("Alice", 30);
console.WriteLine(person1.Name); // Output: Alice
console.WriteLine(person1.Age); // Output: 30

var person2 = new person("Alice", 30);
Console.WriteLine(person1 == person2); // Output: True
```

### With Expressions
- You can create a new record instance based on an existing one using the with expression.

```csharp
var person1 = new person("Alice", 30);
var person2 = person1 with { Age = 60 };
Console.WriteLine(person2.Age); // Output: 60
```

### Use Cases for Records
- Records are ideal for representing data models(Results, Configurations) (Class that primarily hold data).
- It's good alternative to tuples when you want to give meaningful names to the data fields.
- So you can use with dictionarirs, lists, or hashsets to store collections of records.
- **Keep in mind that when you compare two records, the comparison is based on their values, not their references. So only use record for immutable data.**

### Difference between Record, Class, and Struct

- **Class**: Classes are reference types with reference semantics. You operate on them at the reference level because you care about the particular instance.
- **Struct**: Structs are value types with value semantics. You operate on them at the value level because you care about the data they hold.
- **Record**: Records are reference types with value semantics. You operate on them at the value level because you care about the data they hold, not the particular instance.


## Delegates

### What is a Delegate?
- Delegate is a variable that can hold a reference to a method.
- They allow you to pass methods as parameters.

### How to use Delegates?

```csharp
var calculator = new Calculator();

var squareValue= calculator.PerformOperation(5, x => x * x);
Console.WriteLine(squareValue);

var cubeValue= calculator.PerformOperation(5, x => x * x * x);
Console.WriteLine(cubeValue);

public class Calculator
{
    public delegate int NumberOperation(int x);

    public int PerformOperation(int x, NumberOperation operation)
    {
        return operation(x);
    }
}
```
### Action Delegate
- Action delegate is a predefined delegate type that represents a method that takes parameters but does not return a value.
- You can use Action<T> for 1 parameter, Action<T1, T2> for 2 parameters, and so on up to 16 parameters.

```csharp
var calculator = new Calculator();
calculator.PrintMessage("Hello from Action delegate!", msg => Console.WriteLine(msg));
public class Calculator
{
    public void PrintMessage(string message, Action<string> messageOperation)
    {
        messageOperation(message);
    }
}
```

### Function Delegate
- Func delegate is a predefined delegate type that represents a method that 
takes parameters and returns a value.
- You can use Func<T, TResult> for 1 parameter, Func<T1, T2, TResult> for 2 parameters, and so on up to 16 parameters plus return type.

```csharp
public int PerformOperation(int x, Func<int,int> operation)
{
    return operation(x);
}
```
### Predicate Delegate
- Predicate delegate is a predefined delegate type that represents a method that takes a single parameter and returns a boolean value.

```csharp
var calculator = new Calculator();
var isEven = calculator.PerformCheck(4, x => x % 2 == 0);
Console.WriteLine($"Is 4 even? {isEven}");
public class Calculator
{
    public bool PerformCheck(int x, Predicate<int> check)
    {
        return check(x);
    }
}
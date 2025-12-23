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


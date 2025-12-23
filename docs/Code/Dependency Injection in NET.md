---
title: Dependency Injection in NET
---

# Dependency Injection in .NET by Mark Seemann

## Chapter 1 : Dependency Injection Tasting Menu

<aside>
📖 Dependency Injection is a set of software design principles and patterns that enable us to develop loosely coupled code.

</aside>

The software development Q&A website Stack Overflow features an answer to the question How to explain Dependency Injection to a 5-year old. The most highly rated answer, provided by John Munsch,1 provides a surprisingly accurate analogy targeted at the (imaginary) five-year-old inquisitor

> When you go and get things out of the refrigerator for yourself, you can cause problems. You might leave the door open, you might get something Mommy or Daddy doesn't want you to have. You might even be looking for something we don't even have or which has expired.
What you should be doing is stating a need, "I need something to drink with lunch," and then
> 

> Dependency injections is when you have something setting the dependencies for you. This something is usually a framework
> 

[How to explain dependency injection to a 5-year-old?](https://stackoverflow.com/questions/1638919/how-to-explain-dependency-injection-to-a-5-year-old)

### Writing maintainable code

Ultimately, the purpose of most programming techniques is to deliver working software as efficiently as possible. One aspect of that is to write maintainable code.

Unless you write prototypes or applications that never make it past release 1, you’ll soon find yourself maintaining and extending existing code bases. To be able to work effectively with such a code base, it must be as maintainable as possible.

One of many ways to make code maintainable is through loose coupling. Loose coupling makes code extensible, and extensibility makes it maintainable. DI is nothing more than a technique that enables loose coupling.

> Program to an interface, not an implementation-GOF
> 

### Understanding the purpose of DI

Loose coupling can make a code base much more maintainable.

That’s the easy part. Programming to an interface instead of an implementation is easy. The question is, where do the instances come from? In a sense, this is what this entire book is about.

### Hello DI

### Hello DI code

### COLLABORATORS

To get a sense of the structure of the program, we’ll start by looking at the Main method of the console application, and then I’ll show you the collaborating classes.

```csharp
private static void Main()
{
 IMessageWriter writer = new ConsoleMessageWriter();
 var salutation = new Salutation(writer);
 salutation.Exclaim();
}
```

The program needs to write to the console, so it creates a new instance of ConsoleMessageWriter that encapsulates exactly that functionality. It passes that message writer to the Salutation class so that the salutation instance knows where to write its messages. Because everything is now wired up properly, you can execute the logic, which results in the message being written to the screen.

```csharp
public class Salutation
{
     private readonly IMessageWriter writer;
// Step 1 : Inject Dependency
     public Salutation(IMessageWriter writer)
     {
             if (writer == null)
             {
             throw new ArgumentNullException("writer");
             }
             this.writer = writer;
     }
// Step 2 : Use Dependency
     public void Exclaim()
     {
         this.writer.Write("Hello DI!");
     }
}
```

```csharp
public class ConsoleMessageWriter : IMessageWriter
{
 public void Write(string message)
 {
 Console.WriteLine(message);
 }
}
```

The ConsoleMessageWriter class implements IMessageWriter by wrapping the Base Class Library’s Console class.

### Benefits of DI

How is the previous example better than the usual single line of code we normally use to implement Hello World in C#? In this example, DI adds an overhead of 1,100%, but, as complexity increases from one line of code to tens of thousands, this overhead diminishes and all but disappears.

### What to inject and what not to inject

If you’re convinced that loose coupling is a good idea, you may want to make everything loosely coupled. Overall, this is a good idea. When you must decide how to package modules, loose coupling provides especially useful guidance. You don’t have to abstract everything away and make everything pluggable. In this section, I’ll provide you with some decision tools that can help you decide how to model your DEPENDENCIES

### Seams

Everywhere we decide to program against an interface instead of a concrete type, we introduce a SEAM into the application. A SEAM is a place where an application is assembled from its constituent parts, similar to the way a piece of clothing is sewn together at its seams. It’s also a place where we can disassemble the application and work with the modules in isolation.

The Hello DI sample I built in section 1.2 contains a SEAM between Salutation and ConsoleMessageWriter, as illustrated in figure 1.12. The Salutation class doesn’t directly depend on the ConsoleMessageWriter class; rather, it uses the IMessageWriter interface to write messages. You can take the application apart at this SEAM and reassemble it with a different message writer

As you learn DI, it can be helpful to categorize your dependencies into STABLE DEPENDENCIES and VOLATILE DEPENDENCIES, but deciding where to put your SEAMS will soon become second nature to you.

### Stable Dependencies

The BCL modules are always available to your application, because it needs the .NET Framework to run. The concern about parallel development doesn’t apply to these modules because they already exist, and you can always reuse a BCL library in another application.

By default, you can consider most (but not all) types defined in the BCL as safe, or STABLE DEPENDENCIES—I call them stable because they’re already there, tend to be backwards compatible, and invoking them has deterministic outcomes.

Most STABLE DEPENDENCIES are BCL types, but other dependencies can be stable as well. The important criteria for STABLE DEPENDENCIES are

- The class or module already exists.
- You expect that new versions won’t contain breaking changes.
- The types in question contain deterministic algorithms.
- You never expect to have to replace the class or module with another

Ironically, DI CONTAINERS themselves will tend to be STABLE DEPENDENCIES, because they fit all the criteria. When you decide to base your application on a particular DI CONTAINER, you risk being stuck with this choice for the entire lifetime of the application; that’s yet another reason why you should limit the use of the container to the application’s COMPOSITION ROOT.

### VOLATILE DEPENDENCIES

A DEPENDENCY should be considered Volatile if any of the following criteria is true

- The DEPENDENCY introduces a requirement to set up and configure a runtime environment for the application. A relational database is the archetypical example: if we don’t hide the relational database behind a SEAM, we can never replace it by any other technology. It also makes it hard to set up and run automated unit tests.
- The DEPENDENCY doesn’t yet exist, but is still in development. The obvious symptom of such dependencies is the inability to do parallel development
- The DEPENDENCY isn’t installed on all machines in the development organization. This may be the case for expensive third-party libraries, or dependencies that can’t be installed on all operating systems. The most common symptom is disabled TESTABILITY.
- The dependency contains nondeterministic behavior. This is particularly important in unit tests, because all tests should be deterministic. Typical sources of nondeterminism are random numbers and algorithms that depend on the current date or time.
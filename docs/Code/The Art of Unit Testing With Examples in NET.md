---
title: The Art of Unit Testing With Examples in NET
---

# The Art of Unit Testing: With Examples in .NET by Roy Osherove

## Chapter 0 : Title

## Chapter 1 : The basics of unit testing

### Defining unit testing, step by step

<aside>
📢 **DEFINITION 1.0** A unit test is a piece of a code (usually a method) that invokes another piece of code and checks the correctness of some assumptions afterward. If the assumptions turn out to be wrong, the unit test has failed. A unit is a method or function.

</aside>

You might feel that you’d like to minimize the size of a unit of work being tested. I used to feel that way. But I don’t anymore. I believe if you can create a unit of work that’s larger, and where its end result is more noticeable to an end user of the API, you’re creating tests that are more maintainable. If you try to minimize the size of a unit of work, you end up faking things down the line that aren’t really end results to the user of a public API but instead are just train stops on the way to the main station.

<aside>
📢 **UPDATED DEFINITION 1.0** A unit test is a piece of code that invokes a unit of work and checks one specific end result of that unit of work. If the assumptions on the end result turn out to be wrong, the unit test has failed. A unit test’s scope can span as little as a method or as much as multiple classes.

</aside>

### The importance of writing good unit tests

Most people who try to unit test their code either give up at some point or don’t actually perform unit tests. Instead, either they rely on system and integration tests to be performed much later in the product lifecycle or they resort to manually testing the code via custom test applications or by using the end product they’re developing to invoke their code.

There’s no point in writing a bad unit test, unless you’re learning how to write a good one and these are your first steps in this field. **If you’re going to write a unit test badly without realizing it, you may as well not write it at all and save yourself the trouble it will cause down the road with maintainability and time schedules.** By defining what a good unit test is, you can make sure you don’t start off with the wrong notion of what your objective is.

### We’ve all written unit tests (sort of)

You may be surprised to learn this, but you’ve already implemented some types of unit testing on your own. Have you ever met a developer who has not tested their code before handing it over? Well, neither have I.

You might have used a console application that called the various methods of a class or component, or perhaps some specially created WinForms or Web Forms UI that checked the functionality of that class or component, or maybe even manual tests run by performing various actions within the real application’s UI. The end result is that you’ve made certain, to a degree, that the code works well enough to pass it on to someone else.

### Properties of a good unit test

A unit test should have the following properties:

- It should be automated and repeatable.
- It should be easy to implement.
- It should be relevant tomorrow.
- Anyone should be able to run it at the push of a button.
- It should run quickly.
- It should be consistent in its results (it always returns the same result if you don’t change anything between runs).
- It should have full control of the unit under test.
- It should be fully isolated (runs independently of other tests).
- When it fails, it should be easy to detect what was expected and determine how to pinpoint the problem.

### Integration tests

I consider integration tests as any tests that aren’t fast and consistent and that use one or more real dependencies of the units under test. For example, if the test uses the real system time, the real filesystem, or a real database, it has stepped into the realm of integration testing.

If a test uses the real database, then it’s no longer only running in memory, in that its actions are harder to erase than when using only in-memory fake data. The test will also run longer, again a reality that it has no control over. Unit tests should be fast. **Integration tests are usually much slower. When you start having hundreds of tests, every half-second counts.**

### Drawbacks of nonautomated integration tests compared to automated unit tests

- **Can I run and get results from the test I wrote two weeks or months or years ago?**
    - If you can’t, how would you know whether you broke a feature that you created earlier? Code changes regularly during the life of an application, and if you can’t (or won’t) run tests for all the previously working features after changing your code, you just might break it without knowing. I call this “accidental bugging,” and it seems to occur a lot near the end of a software project, when developers are under pressure to fix existing bugs. Sometimes they introduce new bugs inadvertently as they resolve the old ones. Wouldn’t it be great to know that you broke something within three minutes of breaking it?
    
- **Can any member of my team run and get results from tests I wrote two months ago?**
    - This goes with the previous point but takes it up a notch. You want to make sure that you don’t break someone else’s code when you change something. Many developers fear changing legacy code in older systems for fear of not knowing what other code depends on what they’re changing. In essence, they risk changing the system into an unknown state of stability.
    - Few things are scarier than not knowing whether the application still works, especially when you didn’t write that code. If you knew you weren’t breaking anything, you’d be much less afraid of taking on code you’re less familiar with, because you have that safety net of unit tests.
    - Good tests can be accessed and run by anyone.
- **Can I run all the tests I’ve written in no more than a few minutes?**
    - If you can’t run your tests quickly (seconds are better than minutes), you’ll run them less often (daily or even weekly or monthly in some places). The problem is that when you change code, you want to get feedback as early as possible to see if you broke something. The more time between running the tests, the more changes you make to the system, and the (many) more places to search for bugs when you find that you broke something.
    - Good tests should run quickly
- **Can I run all the tests I’ve written at the push of a button?**
    - If you can’t, it probably means that you have to configure the machine on which the tests will run so that they run correctly (setting connection strings to the database, for example) or that your unit tests aren’t fully automated. If you can’t fully automate your unit tests, you’ll probably avoid running them repeatedly, as will everyone else on your team.
    - Good tests should be easily executed in their original form, not manually.
- **Can I write a basic test in no more than a few minutes?**
    - The harder it is to write a test, the less likely you are to write more tests or to focus on anything other than the “big stuff” that you’re worried about. One of the strengths of unit tests is that they tend to test every little thing that might break, not only the big stuff. People are often surprised at how many bugs they can find in code they thought was simple and bug free.
    - Good tests against the system should be easy and quick to write, once you’ve figured out the patterns you want to use to test your specific object model. Small warning: even experienced unit testers can find that it may take 30 minutes or more to figure out how to write the very first unit test against an object model they’ve never unit tested before. This is part of the work, and is expected. The second and subsequent tests on that object model should be very easy to accomplish.

### What makes unit tests good

<aside>
📢 **DEFINITION 1.2** A unit test is an automated piece of code that invokes the unit of work being tested, and then checks some assumptions about a single end result of that unit. A unit test is almost always written using a unit testing framework. It can be written easily and runs quickly. It’s trustworthy, readable, and maintainable. It’s consistent in its results as long as production code hasn’t changed.

</aside>

### Test-driven development

Many people feel that the best time to write unit tests for software is after the software has been written, but a growing number prefer writing unit tests before the production code is written. This approach is called test-first or test-driven development (TDD).

TDD is different from traditional development, as figure 1.4 shows. You begin by writing a test that fails; then you move on to creating the production code, seeing the test pass, and continuing on to either refactor your code or create another failing test.

## Chapter 2 : A First Unit Test

### Frameworks for unit testing

### What unit testing frameworks offer

- **They weren’t structured.** You had to reinvent the wheel every time you wanted to test a feature. One test might have looked like a console application, another used a UI form, and another used a web form. You didn’t have time to spend on testing, and the tests failed the “easy to implement” requirement.
- **They weren’t repeatable.** Neither you nor your team members could run the tests you’d written in the past. That breaks the “repeatedly” requirement and prevents you from finding regression bugs. With a framework, you can more easily and automatically write tests that are repeatable.
- **They didn’t cover all the important parts of the code.** The tests didn’t test all the code that matters. That means all the code with logic in it, because each and every one of those could contain a potential bug. If it were easier to write the tests, you’d be more inclined to write more of them and get better coverage.

### Writing your first test

How do you test your code? A unit test usually comprises three main actions

- **Arrange** objects, creating and setting them up as necessary.
- **Act** on an object.
- **Assert** that something is as expected.

```csharp
[Test]
 public void IsValidFileName_BadExtension_ReturnsFalse()
 {
 LogAnalyzer analyzer = new LogAnalyzer();
 bool result = analyzer.IsValidLogFileName("filewithbadextension.foo");
 Assert.False(result);
 }
```

<aside>
🔥 Most of Unit Test Code in this book is based on NUnit but you can apply to most of concept to XUnit.

</aside>

### The Assert class

The Assert class has static methods and is located in the NUnit.Framework namespace. It’s the bridge between your code and the NUnit framework, and its purpose is to declare that a specific assumption is supposed to exist.

If the arguments that are passed into the Assert class turn out to be different than what you’re asserting, NUnit will realize the test has failed and will alert you. You can optionally tell the Assert class what message to alert you with if the assertion fails.

```csharp
Assert.AreEqual(2, 1+1, "Math is broken");
Assert.AreSame(expectedObject, actualObject, message);
Assert.AreSame(int.Parse("1"),int.Parse("1"),"this test should fail").
```

also note that all the assert methods take a last parameter of type “string,” which gets displayed in addition to the framework output, in case of a test failure. Please, never, ever, use this parameter (it’s always optional to use). Just make sure your test name explains what’s supposed to happen. Often, people write the trivially obvious things like “test failed” or “expected x instead of y,” which the framework already provides. Much like comments in code, if you have to use this parameter, your method name should be clearer.

### From red to green: passing the tests

NUnit’s GUI is built with a simple idea in mind: all the tests should pass in order to get the green light to go ahead. If even one of the tests fails, you’ll see a red light on the top progress bar to let you know that something isn’t right with the system

The red/green concept is prevalent throughout the unit testing world and especially in test-driven development. Its mantra is `“Red-Green-Refactor”`, **meaning that you start with a failing test, then pass it, and then make your code readable and more maintainable.**

### Ignoring tests

Sometimes you’ll have tests that are broken, and you still need to check in your code to the main source tree. In those rare cases (and they should be rare!), you can put an [Ignore] attribute on tests that are broken because of a problem in the test, not in the code.

```csharp
[Test]
[Ignore("there is a problem with this test")]
 public void IsValidFileName_ValidFile_ReturnsTrue()
 {
 /// ...
 }
```

### Setting test categories

You can set up your tests to run under specific test categories, such as slow tests and fast tests. You do this by using NUnit’s [Category] attribute:

```csharp
[Test]
[Category("Fast Tests")]
 public void IsValidFileName_ValidFile_ReturnsTrue()
 {
 /// ...
 }
```

When you load your test assembly again in NUnit, you can see it organized by categories instead of namespaces. Switch to the Categories tab in NUnit, and double-click the category you’d like to run so that it moves into the lower Selected Categories pane. Then click the Run button.

## Chapter 3 : Using stubs to break dependencies

## Introducing stubs

Flying people into space presents interesting challenges to engineers and astronauts, one of the more difficult being how to make sure the astronaut is ready to go into space and operate all the machinery during orbit. A full integration test for the space shuttle would have required being in space, and that’s obviously not a safe way to test astronauts. That’s why NASA built full simulators that mimicked the surroundings of a space shuttle’s control deck, which removed the external dependency of having to be in outer space.

<aside>
📢 **DEFINITION** An external dependency is an object in your system that your code under test interacts with and over which you have no control. (Common examples are filesystems, threads, memory, time, and so on.)

</aside>

In programming, you use stubs to get around the problem of external dependencies.

<aside>
📢 **DEFINITION** A stub is a controllable replacement for an existing dependency (or collaborator) in the system. By using a stub, you can test your code without dealing with the dependency directly.

</aside>

## Refactoring your design to be more testable

```csharp
public bool IsValidLogFileName(string fileName)
{
 FileExtensionManager mgr =
 new FileExtensionManager();
 return mgr.IsValid(fileName);
}
class FileExtensionManager
 {
 public bool IsValid(string fileName)
 {
 //read some file here
 }
 }
```

Next, you can tell your class under test that instead of using the concrete FileExtensionManager class, it will deal with some form of ExtensionManager, without knowing its concrete implementation. In .NET, this could be accomplished by either using a base class or an interface that FileExtensionManager would extend.

```csharp
public class FileExtensionManager : IExtensionManager
 {
 public bool IsValid(string fileName)
 {
 ...
 }
}
public interface IExtensionManager
 {
 bool IsValid (string fileName);
 }

//the unit of work under test:
public bool IsValidLogFileName(string fileName)
{
 IExtensionManager mgr =  new FileExtensionManager();
 return mgr.IsValid(fileName);
 }
```

You create an interface with one IsValid (string) method and make FileExtensionManager implement that interface. It still works exactly the same way, only now you can replace the “real” manager with your own “fake” manager, which you’ll create later to support your test.

```csharp
public class AlwaysValidFakeExtensionManager:IExtensionManager
 {
 public bool IsValid(string fileName)
 {
 return true;
 }
 }
```

This fake extension manager will always return true, so name the class `AlwaysValidFakeExtensionManager`, so that the reader of your future test will understand what will be the behavior of the fake object, without needing to read its source code.

By saying that an object or a variable is fake, you delay deciding how to name this look-alike object and remove any confusion that would have resulted from naming it mock or stub extension manager.

Now you have an interface and two classes implementing it, but your method under test still calls the real implementation directly:

```csharp
public bool IsValidLogFileName(string fileName)
 {
 IExtensionManager mgr = new FileExtensionManager();
 return mgr. IsValid (fileName);
 }
```

You somehow have to tell your method to talk to your implementation rather than the original implementation of `IExtensionManager`. You need to introduce a seam into the code, where you can plug in your stub.

## Dependency injection: inject a fake implementation into a unit under test

There are several proven ways to create interface-based seams in your code—places where you can inject an implementation of an interface into a class to be used in its methods. Here are some of the most notable ways:

- Receive an interface at the constructor level and save it in a field for later use
- Receive an interface as a property get or set and save it in a field for later use
- Receive an interface just before the call in the method under test using one of the following
    - A parameter to the method (parameter injection)
    - A factory class
    - A local factory method
    - Variations on the preceding techniques

## Inject a fake at the constructor level (constructor injection)

```csharp
public class LogAnalyzer
{
 private IExtensionManager manager;
 public LogAnalyzer(IExtensionManager mgr)
 {
 manager = mgr;
 }
 public bool IsValidLogFileName(string fileName)
 {
 return manager.IsValid(fileName);
 }
 public interface IExtensionManager
 {
 bool IsValid(string fileName);
 }
 [TestFixture]
 public class LogAnalyzerTests
 {
 [Test]
 public void
 IsValidFileName_NameSupportedExtension_ReturnsTrue()
 {
 	 FakeExtensionManager myFakeManager =
 	 new FakeExtensionManager();
 	 myFakeManager.WillBeValid = true;
 	 LogAnalyzer log =
 	 new LogAnalyzer (myFakeManager);
 	 bool result = log.IsValidLogFileName("short.ext");
 	 Assert.True(result);
 }
 }
 internal class FakeExtensionManager : IExtensionManager
 {
 	 public bool WillBeValid = false;
 	 public bool IsValid(string fileName)
 	 {
 	 return WillBeValid;
 	 }
 }
```

### CAVEATS WITH CONSTRUCTOR INJECTION

Problems can arise from using constructors to inject implementations. If your code under test requires more than one stub to work correctly without dependencies, adding more and more constructors (or more and more constructor parameters) becomes a hassle, and it can even make the code less readable and less maintainable.

Suppose LogAnalyzer also had a dependency on a web service and a logging service in addition to the file extension manager. The constructor might look like this

```csharp
public LogAnalyzer(IExtensionManager mgr, ILog logger, IWebService service)
{
 // this constructor can be called by tests
 manager = mgr;
 log= logger;
 svc= service;
}
```

One solution is to create a special class that contains all the values needed to initialize a class and to have only one parameter to the method: that class type. That way, you only pass around one object with all the relevant dependencies

Another possible solution is using inversion of control (IoC) containers. You can think of IoC containers as “smart factories” for your objects (although they’re much more than that). A few well-known containers of this type are Microsoft Unity, StructureMap, and Castle Windsor. They provide special factory methods that take in the type of object you’d like to create and any dependencies that it needs and then initialize the object using special configurable rules such as what constructor to call, what properties to set in what order, and so on. They’re powerful when put to use on a complicated composite object hierarchy where creating an object requires creating and initializing objects several levels down the line. If your class needs an ILogger interface at its constructor, for example, you can configure such a container object to always return the same ILogger object that you give it, when resolving this interface requirement. The end result of using containers is usually simpler handling and retrieving of objects and less worry about the dependencies or maintaining the constructors.

<aside>
🔥 TIP There are many other successful container implementations, such as Autofac or Ninject, so look at them when you read more about this topic. Dealing with containers is beyond the scope of this book, but you can start reading about them with Scott Hanselman’s list at www.hanselman.com/ blog/ListOfNETDependencyInjectionContainersIOC.aspx. To really get a grasp on this topic in a deeper way, I recommend Dependency Injection in .NET (Manning Publications, 2011) by Mark Seeman. A

</aside>

### WHEN YOU SHOULD USE CONSTRUCTOR INJECTION

My experience is that using constructor arguments to initialize objects can make my testing code more cumbersome unless I’m using helper frameworks such as IoC containers for object creation. But it’s my preferred way, because it sucks the least in terms of having APIs that are readable and understandable.

### CAVEATS WITH CONSTRUCTOR INJECTION

Problems can arise from using constructors to inject implementations. If your code under test requires more than one stub to work correctly without dependencies, adding more and more constructors (or more and more constructor parameters) becomes a hassle, and it can even make the code less readable and less maintainable.

## Simulating exceptions from fakes

Here’s a simple example of how you can make your fake class configurable to throw an exception, so that you can simulate any type of exception when a method is invoked. For the sake of argument let’s say that you’re testing the following requirement: if the file extension manager throws an exception, you should return false but not bubble up the exception

```csharp
[Test]
 public void
 IsValidFileName_ExtManagerThrowsException_ReturnsFalse()
 {
 FakeExtensionManager myFakeManager =
 new FakeExtensionManager();
myFakeManager.WillThrow = new Exception(“this is fake”);
 LogAnalyzer log =
 new LogAnalyzer (myFakeManager);
 bool result = log.IsValidLogFileName("anything.anyextension");
 Assert.False(result);
 }
 }
 internal class FakeExtensionManager : IExtensionManager {
 public bool WillBeValid = false;;
 public Exception WillThrow = null ;
 public bool IsValid(string fileName)
 {
 if(WillThrow !=null)
 { throw WillThrow;}
 return WillBeValid;
 }
 }
```

## Using Extract and Override to create fake results

```csharp
public class LogAnalyzerUsingFactoryMethod
 {
 public bool IsValidLogFileName(string fileName)
 {
 return this.IsValid(fileName);
 }
 protected virtual bool IsValid(string fileName)
 {
 FileExtensionManager mgr = new FileExtensionManager();
 return mgr.IsValid(fileName);
 }
 }

[Test]
 public void overrideTestWithoutStub()
 {
 TestableLogAnalyzer logan = new TestableLogAnalyzer();
 logan.IsSupported = true;
 bool result = logan.IsValidLogFileName("file.ext");
 Assert.True(result,"...");
 }

class TestableLogAnalyzer: LogAnalyzerUsingFactoryMethod
 {
 public bool IsSupported;
 protected override bool IsValid(string fileName)
 {
 return IsSupported;
 }
 }

## Overcoming the encapsulation problem

Some people feel that opening up the design to make it more testable is a bad thing because it hurts the object-oriented principles the design is based on.

I can wholeheartedly say to those people, “Don’t be silly.” Object-oriented techniques are there to enforce constraints on the end user of the API (the end user being the programmer who will use your object model) so that the object model is used properly and is protected from unintended usage.

## Using internal and [InternalsVisibleTo]

If you dislike adding a public constructor that everyone can see to your class, you can make it internal instead of public. You can then expose all internal related members and methods to your test assembly by using the [InternalsVisibleTo] assemblylevel attribute.

```csharp
public class LogAnalyzer
 {
 ...
 internal LogAnalyzer (IExtensionManager extentionMgr)
 {
 manager = extentionMgr;
 }
 ...
}
using System.Runtime.CompilerServices;
[assembly:
 InternalsVisibleTo("AOUT.CH3.Logan.Tests")]
```

## Using the [Conditional] attribute

The System.Diagnostics.ConditionalAttribute is notable for its nonintuitive action. When you put this attribute on a method, you initialize the attribute with the string signifying a conditional build parameter that’s passed in as part of the build. (DEBUG and RELEASE are the two most common ones, and Visual Studio uses them by default according to your build type.)

```csharp
[Conditional ("DEBUG")]
public void DoSomething()
{
}
```

<aside>
👉🏿 NOTE These annotated methods won’t be hidden from the production code, which is different from how the next technique we’ll discuss behaves.

</aside>

## Using #if and #endif with conditional compilation

```csharp
#if DEBUG
 public LogAnalyzer (IExtensionManager extensionMgr)
 {
 manager = extensionMgr;
 }
#endif
...
#if DEBUG
 [Test]
 public void
IsValidFileName_SupportedExtension_True()
 {
...
 //create analyzer and inject stub
 LogAnalyzer log =
 new LogAnalyzer (myFakeManager);
 ...
 }
#endif
```

## Summary

A stub can be injected into your code in many different ways. The real trick is to locate the right layer of indirection, or to create one, and then use it as a seam from which you can inject your stub into running code.

We call these classes fake because we don’t want to commit to them only being used as stubs or as mocks.

The deeper you go down the layers of interactions, the harder it will be to understand the test and to understand the code under test and its deep interactions with other objects. The closer you are to the surface of the object under test, the easier your test will be to understand and manage, but you may also be giving up some of your power to manipulate the environment of the object under test.

Learn the different ways of injecting a stub into your code. When you master them, you’ll be in a much better position to pick and choose which method you want to use when.
# The Art of Unit Testing: With Examples in .NET by Roy Osherove
 
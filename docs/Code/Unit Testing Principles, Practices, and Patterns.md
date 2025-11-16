# Unit Testing: Principles, Practices, and Patterns by Vladimir Khorikov

## Chapter 1 : The Goal of Unit Testing

## The current state of unit testing

When it comes to enterprise application development, almost every project includes at least some unit tests. A significant percentage of such projects go far beyond that: they achieve good code coverage with lots and lots of unit and integration tests. The ratio between the production code and the test code could be anywhere between 1:1 and 1:3 (for each line of production code, there are one to three lines of test code). Sometimes, this ratio goes much higher than that, to a whopping 1:10.

## The goal of unit testing

### **The relationship between unit testing and code design**

- The ability to unit test a piece of code is a nice litmus test, but it only works in one direction. Its a good negative indicatorit points out poor-quality code with relatively high accuracy. If you find that code is hard to unit test, its a strong sign that the code needs improvement. The poor quality usually manifests itself in tight coupling, which means different pieces of production code are not decoupled from each other enough, and its hard to test them separately.

>  What is the goal of unit testing, then? The goal is to **enable sustainable growth of the software project.** The term sustainable is key. Its quite easy to grow a project, especially when you start from scratch. Its much harder to sustain this growth over time.

## Chapter 2 : What is Unit test?

## The definition of unit test

There are a lot of definitions of a unit test. Stripped of their non-essential bits, the definitions all have the following three most important attributes. A unit test is an automated test that

- Verifies a small piece of code (also known as a unit)
- Does it quickly,
- And does it in an isolated manner

>  DEFINITION A method under test (MUT) is a method in the SUT called by the test. The terms MUT and SUT are often used as synonyms, but normally, MUT refers to a method while SUT refers to the whole class.

>  DEFINITION A mock is a special kind of test double that allows you to examine interactions between the system under test and its collaborators.

- Test double is an overarching term that describes all kinds of non-productionready, fake dependencies in a test.
- Mock is just one kind of such dependencies.

## Tests written using the classical style of unit testing

```csharp
[Fact]
public void Purchase_succeeds_when_enough_inventory()
{
// Arrange
var store = new Store();
store.AddInventory(Product.Shampoo, 10);
var customer = new Customer();
// Act
bool success = customer.Purchase(store, Product.Shampoo, 5);
// Assert
Assert.True(success);
Assert.Equal(5, store.GetInventory(Product.Shampoo));
}
```

## Chapter 3 : The anatomy of Unit Test

## How to structure a unit test

### Using the AAA pattern

The AAA pattern advocates for splitting each test into three parts: arrange, act, and assert. (This pattern is sometimes also called the 3A pattern.) Lets take a Calculator class with a single method that calculates a sum of two numbers:

```csharp
public class Calculator
{
public double Sum(double first, double second)
{
return first + second;
}
}
```

```csharp
public class CalculatorTests
{
[Fact]
public void Sum_of_two_numbers()
{
// Arrange
double first = 10;
double second = 20;
var calculator = new Calculator();
// Act
double result = calculator.Sum(first, second);
// Assert
Assert.Equal(30, result);
}
}
```

## Chapter 4 : The four pillars of a good unit test

## Diving into the four pillars of a good unit test

### The first pillar: Protection against regressions

> a regression is a software bug. Its when a feature stops working as intended after some code modification, usually after you roll out new functionality.

An unfortunate fact of programming life is that **code is not an asset, its a liability. The larger the code base, the more exposure it has to potential bugs**. Thats why its crucial to develop a good protection against regressions. Without such protection, you wont be able to sustain the project growth in a long runyoull be buried under an ever-increasing number of bugs.

## Chapter 5 : Mocks and test fragility

## Differentiating mocks from stubs

### The types of test doubles

A test double is an overarching term that describes all kinds of non-production-ready, fake dependencies in tests. The term comes from the notion of a stunt double in a movie. The major use of test doubles is to facilitate testing; they are passed to the system under test instead of real dependencies, which could be hard to set up or maintain.

According to Gerard Meszaros, there are five variations of test doubles: dummy, stub, spy, mock, and fake. Such a variety can look intimidating, but in reality, they can all be grouped together into just two types: mocks and stubs.

## Chapter 6 : Styles of Unit Testing

## The three styles of unit testing

As I mentioned in the chapter introduction, there are three styles of unit testing

- Output-based testing
- State-based testing
- Communication-based testing

## Refactoring toward valuable unit tests

### Identifying the code to refactor

Its rarely possible to significantly improve a test suite without refactoring the underlying code. Theres no way around ittest and production code are intrinsically connected. In this section, youll see how to categorize your code into the four types in order to outline the direction of the refactoring.

## The four types of code

All production code can be categorized along two dimensions

- Complexity or domain significance
- The number of collaborators

**Code complexity** is defined by the number of decision-making (branching) points in the code. The greater that number, the higher the complexity.

**Domain significance** shows how significant the code is for the problem domain of your project. Normally, all code in the domain layer has a direct connection to the end users goals and thus exhibits a high domain significance. On the other hand, utility code doesnt have such a connection.

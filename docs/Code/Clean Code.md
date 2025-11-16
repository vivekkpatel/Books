# Clean Code: A Handbook of Agile Software Craftsmanship by Robert C. Martin

---

## Chapter 1: Clean Code

---

## Chapter 2: Meaningful Names

- **Use intention reveling names**
- **Use Pronounceable names**
    - Humans are good at words. A signiﬁcant part of our brains is dedicated to the concept of words. And words are, by deﬁnition, pronounceable. It would be a shame not to take advantage of that huge portion of our brains that has evolved to deal with spoken language. So make your names pronounceable.
- **Use Searchable Names**
- **Don't use prefix or Suffix**
    - You also don't need to preﬁx member variables with m_ anymore. Your classes and functions should be small enough that you don't need them. And you should be using an editing environment that highlights or colorizes members to make them distinct.
- **Avoid Mental Mapping**
    - Readers shouldn't have to mentally translate your names into other names they already know. This problem generally arises from a choice to use neither problem domain terms nor solution domain terms
    - One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king. Professionals use their powers for good and write code that others can understand.
- **Class Name**
    - Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words like Manager, Processor, Data, or Info in the name of a class. A class name should not be a verb.
- **Method Names**
    - Methods should have verb or verb phrase names like postPayment, deletePage, or save.
- **Don't be Cute**
    - If names are too clever, they will be memorable only to people who share the author's sense of humor, and only as long as these people remember the joke. Will they know what the function named HolyHandGrenade is supposed to do? Sure, it's cute, but maybe in this case DeleteItems might be a better name.
    Choose clarity over entertainment value

---

## Chapter 3: Functions

### Small

- The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

### Blocks and Indenting

- This implies that the blocks within if statements, else statements, while statements, and so on should be one line long. Probably that line should be a function call. Not only does this keep the enclosing function small, but it also adds documentary value because the function called within the block can have a nicely descriptive name.
- This also implies that functions should not be large enough to hold nested structures. Therefore, the indent level of a function should not be greater than one or two. This, of course, makes the functions easier to read and understand.

### Do One Thing

> FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY.

### Switch Statements

- It's hard to make a small switch statement. Even a switch statement with only two cases is larger than I'd like a single block or function to be. It's also hard to make a switch statement that does one thing. By their nature, switch statements always do N things.
- Unfortunately we can't always avoid switch statements, but we can make sure that each switch statement is buried in a low-level class and is never repeated. We do this, of course, with polymorphism.

### Function Arguments

- The ideal number of arguments for a function is zero (niladic). Next comes one (monadic), followed closely by two (dyadic). Three arguments (triadic) should be avoided where possible. More than three (polyadic) requires very special justification—and then shouldn't be used anyway.

##### Flag Arguments

- Flag arguments are ugly. Passing a Boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!

#### Monadic Functions

**Function with One Argument**

- There are two very common reasons to pass a single argument into a function. You may be asking a question about that argument, as in `boolean fileExists("MyFile")`. Or you may be operating on that argument, transforming it into something else and returning it. For example, `InputStream fileOpen("MyFile")` transforms a file name String into an `InputStream` return value. These two uses are what readers expect when they see a function. You should choose names that make the distinction clear, and always use the two forms in a consistent context.

#### Dyadic Functions

**A function with two arguments**

- A function with two arguments is harder to understand than a monadic function. For example, `writeField(name)` is easier to understand than `writeField(output-Stream, name)`.
- Though the meaning of both is clear, the ﬁrst glides past the eye, easily depositing its meaning. The second requires a short pause until we learn to ignore the ﬁrst parameter.
- And that, of course, eventually results in problems because **we should never ignore any part of code. The parts we ignore are where the bugs will hide.**
- There are times, of course, where two arguments are appropriate. For example, Point p = new Point(0,0); is perfectly reasonable. Cartesian points naturally take two arguments. Indeed, we'd be very surprised to see new Point(0). However, the two arguments in this case are ordered components of a single value! Whereas output-Stream and name have neither a natural cohesion, nor a natural ordering.
- Even obvious dyadic functions like `assertEquals(expected, actual)` are problematic. How many times have you put the actual where the expected should be? The two arguments have no natural ordering. The expected, actual ordering is a convention that requires practice to learn.
- Dyads aren't evil, and you will certainly have to write them. However, you should be aware that they come at a cost and should take advantage of what mechanism may be available to you to convert them into monads. For example, you might make the `writeField` method a member of `outputStream` so that you can say `outputStream. writeField(name)`.

#### Triad Functions

**Functions that take three arguments**

- Functions that take three arguments are signiﬁcantly harder to understand than dyads. The issues of ordering, pausing, and ignoring are more than doubled. I suggest you think very carefully before creating a triad.
- For example, consider the common overload of `assertEquals` that takes three arguments: `assertEquals(message, expected, actual)`. How many times have you read the message and thought it was the expected? I have stumbled and paused over that particular triad many times.

#### Argument Objects

When a function seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Consider, for example, the difference between the two following declarations:

```
Circle makeCircle(double x, double y, double radius); 
Circle makeCircle(Point center, double radius); 

```

Reducing the number of arguments by creating objects out of them may seem like cheating, but it's not. When groups of variables are passed together, the way x and y are in the example above, they are likely part of a concept that deserves a name of its own.

#### Verbs and Keywords

Choosing good names for a function can go a long way toward explaining the intent of the function and the order and intent of the arguments. In the case of a monad, the function and argument should form a very nice verb/noun pair. For example, `write(name)` is very evocative. Whatever this "name" thing is, it is being "written." An even better name might be `writeField(name)`, which tells us that the "name" thing is a "field.

#### Output Arguments

Arguments are most naturally interpreted as inputs to a function.

For example:

```
appendFooter(s); 

```

Does this function append s as the footer to something? Or does it append some footer to s? Is s an input or an output?

In the days before object oriented programming it was sometimes necessary to have output arguments. However, much of the need for output arguments disappears in OO languages because this is intended to act as an output argument. In other words, it would be better for `appendFooter` to be invoked as

```
report.appendFooter();

```

> In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.

#### Command Query Separation

**Functions should either do something or answer something, but not both.** Either your function should change the state of an object, or it should return some information about that object. Doing both often leads to confusion.

### Conclusion

> Master programmers think of systems as stories to be told rather than programs to be written.
> 

They use the facilities of their chosen programming language to construct a much richer and more expressive language that can be used to tell that story. Part of that domain-speciﬁc language is the hierarchy of functions that describe all the actions that take place within that system. In an artful act of recursion those actions are written to use the very domain-speciﬁc language they deﬁne to tell their own small part of the story.

This chapter has been about the mechanics of writing functions well. If you follow the rules herein, your functions will be short, well named, and nicely organized. But never forget that your real goal is to tell the story of the system, and that the functions you write need to fit cleanly together into a clear and precise language to help you with that telling.

---

## Chapter 4: Comments

The proper use of comments is to compensate for our failure to express our self in code. **Comments are always failures. We must have them because we cannot always figure out how to express ourselves without them**, but their use is not a cause for celebration.

So when you find yourself in a position where you need to write a comment, think it through and see whether there isn't some way to turn the tables and express yourself in code. **Every time you express yourself in code, you should pat yourself on the back. Every time you write a comment, you should grimace and feel the failure of your ability of expression.**

Why am I so down on comments? Because they lie. Not always, and not intentionally, but too often. The older a comment is, and the farther away it is from the code it describes, the more likely it is to be just plain wrong. The reason is simple. Programmers can't realistically maintain them.

### Explain Yourself in Code

There are certainly times when code makes a poor vehicle for explanation. Unfortunately, many programmers have taken this to mean that code is seldom, if ever, a good means for explanation. This is patently false. Which would you rather see? This:

```
// Check to see if the employee is eligible for full benefits 
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) 

```

Or this?

```
if (employee.isEligibleForFullBenefits()) 

```

It takes only a few seconds of thought to explain most of your intent in code. In many cases it's simply a matter of creating a function that says the same thing as the comment you want to write.

### Good Comments

Some comments are necessary or beneficial. We'll look at a few that I consider worthy of the bits they consume. Keep in mind, however, that the only truly good comment is the comment you found a way not to write.

#### Legal Comments

Sometimes our corporate coding standards force us to write certain comments for legal reasons. For example, copyright and authorship statements are necessary and reasonable things to put into a comment at the start of each source ﬁle.

Here, for example, is the standard comment header that we put at the beginning of every source ﬁle in FitNesse.

```
// Copyright (C) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.
// Released under the terms of the GNU General Public License version 2 or later.

```

### Informative Comments

It is sometimes useful to provide basic information with a comment.

```
// Returns an instance of the Responder being tested. 
protected abstract Responder responderInstance();

```

A comment like this can sometimes be useful, but it is better to use the name of the function to convey the information where possible.

#### Clarification

Sometimes it is just helpful to translate the meaning of some obscure argument or return value into something that's readable. **In general it is better to find a way to make that argument or return value clear in its own right; but when its part of the standard library, or in code that you cannot alter, then a helpful clarifying comment can be useful.**

```
public void testCompareTo() throws Exception
{
    WikiPagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA.PageB");
    WikiPagePath b = PathParser.parse("PageB");
    WikiPagePath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB.PageB");
    WikiPagePath ba = PathParser.parse("PageB.PageA");

    assertTrue(a.compareTo(a) == 0);    // a == a 
    assertTrue(a.compareTo(b) != 0);    // a != b 
    assertTrue(ab.compareTo(ab) == 0);  // ab == ab 
    assertTrue(a.compareTo(b) == -1);   // a < b 
    assertTrue(aa.compareTo(ab) == -1); // aa < ab 
    assertTrue(ba.compareTo(bb) == -1); // ba < bb 
    assertTrue(b.compareTo(a) == 1);    // b > a 
    assertTrue(ab.compareTo(aa) == 1);  // ab > aa 
    assertTrue(bb.compareTo(ba) == 1);  // bb > ba
}

```

### Warning of Consequences

Sometimes it is useful to warn other programmers about certain consequences. For example, here is a comment that explains why a particular test case is turned off:

```
// Don't run unless you 
// have some time to kill. 
public void _testWithReallyBigFile() 
{ 
    writeLinesToFile(10000000);
	response.setBody(testFile); 
    response.readyToSend(this); 
    String responseString = output.toString(); 
    assertSubString("Content-Length: 1000000000", responseString);
    assertTrue(bytesSent > 1000000000);
} 

```

Nowadays, of course, we'd turn off the test case by using the @Ignore attribute with an appropriate explanatory string. @Ignore("Takes too long to run").

### Bad Comments

Most comments fall into this category. Usually they are crutches or excuses for poor code or justifications for insufficient decisions, amounting to little more than the programmer talking to himself.

#### Redundant Comments

```
// Utility method that returns when this.closed is true. Throws an exception 
// if the timeout is reached. 
public synchronized void waitForClose(final long timeoutMillis)
throws Exception 
{ 
    if(!closed)
    { 
        wait(timeoutMillis); 
        if(!closed) 
            throw new Exception("MockResponseSender could not be closed");
    } 
}

```

Above code shows a simple function with a header comment that is completely redundant. The comment probably takes longer to read than the code itself.

#### Mandated Comments

It is just plain silly to have a rule that says that every function must have a javadoc, or every variable must have a comment. Comments like this just clutter up the code, propagate lies, and lend to general confusion and disorganization

```
 /**
 *
 * @param title The title of the CD
 * @param author The author of the CD
 * @param tracks The number of tracks on the CD
 * @param durationInMinutes The duration of the CD in minutes   
 */  
public void addCD(String title, String author, 
                  int tracks, int durationInMinutes) 
{    
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = duration;
    cdList.add(cd);
}

```

### Noise Comments

Sometimes you see comments that are nothing but noise. They restate the obvious and provide no new information.

```
/**
* Default constructor.
*/ 
protected AnnualDateRule() { }

```

---

## Chapter 5: Formatting

> If instead they see a scrambled mass of code that looks like it was written by a bevy of drunken sailors, then they are likely to conclude that the same inattention to detail pervades every other aspect of the project.

### Newspaper Metaphor

Think of a well-written newspaper article. You read it vertically. At the top you expect a headline that will tell you what the story is about and allows you to decide whether it is something you want to read. The first paragraph gives you a synopsis of the whole story, hiding all the details while giving you the broad-brush concepts. As you continue downward, the details increase until you have all the dates, names, quotes, claims, and other minutia.

We would like a source file to be like a newspaper article. The name should be simple but explanatory. The name, by itself, should be sufficient to tell us whether we are in the right module or not. The topmost parts of the source file should provide the high-level concepts and algorithms. Detail should increase as we move downward, until at the end we find the lowest level functions and details in the source file.

### Team Rules

The title of this section is a play on words. Every programmer has his own favorite formatting rules, but if he works in a team, then the team rules.

A team of developers should agree upon a single formatting style, and then every member of that team should use that style. We want the software to have a consistent style. We don't want it to appear to have been written by a bunch of disagreeing individuals.

When I started the FitNesse project back in 2002, I sat down with the team to work out a coding style. This took about 10 minutes. We decided where we'd put our braces, what our indent size would be, how we would name classes, variables, and methods, and so forth. Then we encoded those rules into the code formatter of our IDE and have stuck with them ever since. These were not the rules that I prefer; they were rules decided by the team. As a member of that team I followed them when writing code in the FitNesse project.

---

## Chapter 6: Objects and Data Structures

There is a reason that we keep our variables private. We don't want anyone else to depend on them. We want to keep the freedom to change their type or implementation on a whim or an impulse. Why, then, do so many programmers automatically add getters and setters to their objects, exposing their private variables as if they were public?

### Data/Object Anti-Symmetry

<aside>
💡 These two examples show the difference between objects and data structures. Objects hide their data behind abstractions and expose functions that operate on that data. Data structure expose their data and have no meaningful functions.

</aside>

Consider, for example, the procedural shape example in the Geometry class operates on the three shape classes. The shape classes are simple data structures without any behavior. All the behavior is in the Geometry class.

```
public class Square
    {  
        public Point topLeft;
        public double side;
    }
public class Rectangle 
    {  
        public Point topLeft;
        public double height;
        public double width;
    }
public class Circle
    { 
        public Point center;
        public double radius;
    }  
public class Geometry 
{ 
    public final double PI = 3.141592653589793;
    public double area(Object shape) throws NoSuchShapeException
        { 
            if (shape instanceof Square)
            {     
            Square s = (Square)shape; 
            return s.side * s.side;   
            }
            else if (shape instanceof Rectangle)
            {      
            Rectangle r = (Rectangle)shape;    
            return r.height * r.width; 
            }    
            else if (shape instanceof Circle) 
            {      
            Circle c = (Circle)shape;   
            return PI * c.radius * c.radius; 
            }   
        throw new NoSuchShapeException();
        } 
}

```

Object-oriented programmers might wrinkle their noses at this and complain that it is procedural—and they'd be right. But the sneer may not be warranted. Consider what would happen if a perimeter() function were added to Geometry. The shape classes would be unaffected! Any other classes that depended upon the shapes would also be unaffected! On the other hand, if I add a new shape, I must change all the functions in Geometry to deal with it. Again, read that over. Notice that the two conditions are diametrically opposed.

Now consider the object-oriented solution in Listing 6-6. Here the area() method is polymorphic. No Geometry class is necessary. So if I add a new shape, none of the existing functions are affected, but if I add a new function all of the shapes must be changed!

```
public class Square implements Shape 
{  
    private Point topLeft;  private double side;
  public double area()
   {    
       return side*side;  
    } 
}
public class Rectangle implements Shape 
{  
    private Point topLeft; 
     private double height;
       private double width;
  public double area() 
  {    
      return height * width;  
    } 
}
public class Circle implements Shape
{ 
    private Point center;
    private double radius;
    public final double PI = 3.141592653589793;
    public double area()
     {    
         return PI * radius * radius;
    } 
}

```

Again, we see the complimentary nature of these two deﬁnitions; they are virtual opposites! This exposes the fundamental dichotomy between objects and data structures:

Again, we see the complimentary nature of these two definitions; they are virtual opposites! This exposes the fundamental dichotomy between objects and data structures:

> Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.

> Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.

In any complex system there are going to be times when we want to add new data types rather than new functions. For these cases objects and OO are most appropriate. On the other hand, there will also be times when we'll want to add new functions as opposed to data types. In that case procedural code and data structures will be more appropriate.

### Conclusion

Objects expose behavior and hide data. This makes it easy to add new kinds of objects without changing existing behaviors. It also makes it hard to add new behaviors to existing objects.

Data structures expose data and have no significant behavior. This makes it easy to add new behaviors to existing data structures but makes it hard to add new data structures to existing functions.

In any given system we will sometimes want the flexibility to add new data types, and so we prefer objects for that part of the system. Other times we will want the flexibility to add new behaviors, and so in that part of the system we prefer data types and procedures. Good software developers understand these issues without prejudice and choose the approach that is best for the job at hand.

---

## Chapter 7: Error Handling

It might seem odd to have a section about error handling in a book about clean code. Error handling is just one of those things that we all have to do when we program. Input can be abnormal and devices can fail. In short, things can go wrong, and when they do, we as programmers are responsible for making sure that our code does what it needs to do.

The connection to clean code, however, should be clear. Many code bases are completely dominated by error handling. When I say dominated, I don't mean that error handling is all that they do. I mean that it is nearly impossible to see what the code does because of all of the scattered error handling. Error handling is important, but if it obscures logic, it's wrong.

### Write Your Try-Catch-Finally Statement First

One of the most interesting things about exceptions is that they define a scope within your program. When you execute code in the try portion of a try-catch-finally statement, you are stating that execution can abort at any point and then resume at the catch.

In a way, try blocks are like transactions. Your catch has to leave your program in a consistent state, no matter what happens in the try. For this reason it is good practice to start with a try-catch-finally statement when you are writing code that could throw exceptions. This helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the try.

### Conclusion

Clean code is readable, but it must also be robust. These are not conflicting goals. We can write robust clean code if we see error handling as a separate concern, something that is viewable independently of our main logic. To the degree that we are able to do that, we can reason about it independently, and we can make great strides in the maintainability of our code.

---

## Chapter 8: Boundaries

We seldom control all the software in our systems. Sometimes we buy third-party packages or use open source. Other times we depend on teams in our own company to produce components or subsystems for us. Somehow we must cleanly integrate this foreign code with our own.

### Using Third-Party Code

There is a natural tension between the provider of an interface and the user of an interface. Providers of third-party packages and frameworks strive for broad applicability so they can work in many environments and appeal to a wide audience. Users, on the other hand, want an interface that is focused on their particular needs. This tension can cause problems at the boundaries of our systems.

Let's look at java.util.Map as an example. Maps have a very broad interface with plenty of capabilities. Certainly this power and flexibility is useful, but it can also be a liability. For instance, our application might build up a Map and pass it around. Our intention might be that none of the recipients of our Map delete anything in the map. But right there at the top of the list is the clear() method. Any user of the Map has the power to clear it. Or maybe our design convention is that only particular types of objects can be stored in the Map, but Maps do not reliably constrain the types of objects placed within them. Any determined user can add items of any type to any Map.

```
 clear() void – Map 
 containsKey(Object key) boolean – Map 
 containsValue(Object value) boolean – Map 
 entrySet() Set – Map 
 equals(Object o) boolean – Map 
 get(Object key) Object – Map 
 getClass() Class<? extends Object> – Object 
 hashCode() int – Map 
 isEmpty() boolean – Map
```

If our application needs a Map of Sensors, you might ﬁnd the sensors set up like this:

```
 Map sensors = new HashMap();
```

Then, when some other part of the code needs to access the sensor, you see this code:

```
 Sensor s = (Sensor)sensors.get(sensorId );
```

We don't just see it once, but over and over again throughout the code. The client of this code carries the responsibility of getting an Object from the Map and casting it to the right type. This works, but it's not clean code. Also, this code does not tell its story as well as it could.

A cleaner way to use Map might look like the following. No user of Sensors would care one bit if generics were used or not. That choice has become (and always should be) an implementation detail.

```
 public class Sensors { 
     private Map sensors = new HashMap();
     public Sensor getById(String id) { 
         return (Sensor) sensors.get(id); 
                                      }
 }
```

**We are not suggesting that every use of Map be encapsulated in this form. Rather, we are advising you not to pass Maps (or any other interface at a boundary) around your system.** If you use a boundary interface like Map, keep it inside the class, or close family of classes, where it is used. Avoid returning it from, or accepting it as an argument to, public APIs.

## **Exploring and Learning Boundaries**

Third-party code helps us get more functionality delivered in less time. Where do we start when we want to utilize some third-party package? It's not our job to test the third-party code, but it may be in our best interest to write tests for the third-party code we use.

Suppose it is not clear how to use our third-party library. We might spend a day or two (or more) reading the documentation and deciding how we are going to use it. Then we might write our code to use the third-party code and see whether it does what we think. We would not be surprised to ﬁnd ourselves bogged down in long debugging sessions trying to ﬁgure out whether the bugs we are experiencing are in our code or theirs.

Learning the third-party code is hard. Integrating the third-party code is hard too. Doing both at the same time is doubly hard. What if we took a different approach? Instead of experimenting and trying out the new stuff in our production code, we could write some tests to explore our understanding of the third-party code. Jim Newkirk calls such tests **learning tests**.

**Learning Tests Are Better Than Free**

The learning tests end up costing nothing. We had to learn the API anyway, and writing those tests was an easy and isolated way to get that knowledge. The learning tests were precise experiments that helped increase our understanding.

Not only are learning tests free, they have a positive return on investment. When there are new releases of the third-party package, we run the learning tests to see whether there are behavioral differences.

Learning tests verify that the third-party packages we are using work the way we expect them to. Once integrated, there are no guarantees that the third-party code will stay compatible with our needs. The original authors will have pressures to change their code to meet new needs of their own. They will ﬁx bugs and add new capabilities. With each release comes new risk. If the third-party package changes in some way incompatible with our tests, we will ﬁnd out right away.

Whether you need the learning provided by the learning tests or not, a clean boundary should be supported by a set of outbound tests that exercise the interface the same way the production code does. Without these boundary tests to ease the migration, we might be tempted to stay with the old version longer than we should.

## **Clean Boundaries**

Interesting things happen at boundaries. Change is one of those things. Good software designs accommodate change without huge investments and rework. When we use code that is out of our control, special care must be taken to protect our investment and make sure future change is not too costly.

Code at the boundaries needs clear separation and tests that deﬁne expectations. We should avoid letting too much of our code know about the third-party particulars. It's better to depend on something you control than on something you don't control, lest it end up controlling you.

We manage third-party boundaries by having very few places in the code that refer to them. We may wrap them as we did with Map, or we may use an ADAPTER to convert from our perfect interface to the provided interface. Either way our code speaks to us better, promotes internally consistent usage across the boundary, and has fewer maintenance points when the third-party code changes.

## Chapter 9 ꞉ Unit Tests

Our profession has come a long way in the last ten years. In 1997 no one had heard of **Test Driven Development**. For the vast majority of us, unit tests were short bits of throwaway code that we wrote to make sure our programs "worked." We would painstakingly write our classes and methods, and then we would concoct some ad hoc code to test them. Typically this would involve some kind of simple driver program that would allow us to manually interact with the program we had written.

### **The Three Laws of TDD**

By now everyone knows that TDD asks us to write unit tests ﬁrst, before we write production code. But that rule is just the tip of the iceberg. Consider the following three laws.

**First Law** You may not write production code until you have written a failing unit test.

**Second Law** You may not write more of a unit test than is sufﬁcient to fail, and not compiling is failing.

**Third Law** You may not write more production code than is sufﬁcient to pass the currently failing test.

These three laws lock you into a cycle that is perhaps thirty seconds long. The tests and the production code are written together, with the tests just a few seconds ahead of the production code.

If we work this way, we will write dozens of tests every day, hundreds of tests every month, and thousands of tests every year. If we work this way, those tests will cover virtually all of our production code. The sheer bulk of those tests, which can rival the size of the production code itself, can present a daunting management problem.

### **Keeping Tests Clean**

Some years back I was asked to coach a team who had explicitly decided that their test code should not be maintained to the same standards of quality as their production code. They gave each other license to break the rules in their unit tests. "Quick and dirty" was the watchword. Their variables did not have to be well named, their test functions did not need to be short and descriptive. Their test code did not need to be well designed and thoughtfully partitioned. So long as the test code worked, and so long as it covered the production code, it was good enough.

What this team did not realize was that having dirty tests is equivalent to, if not worse than, having no tests. The problem is that tests must change as the production code evolves. The dirtier the tests, the harder they are to change. The more tangled the test code, the more likely it is that you will spend more time cramming new tests into the suite than it takes to write the new production code. As you modify the production code, old tests start to fail, and the mess in the test code makes it hard to get those tests to pass again. So the tests become viewed as an ever-increasing liability.

From release to release the cost of maintaining my team's test suite rose. Eventually it became the single biggest complaint among the developers. When managers asked why their estimates were getting so large, the developers blamed the tests. In the end they were forced to discard the test suite entirely.

But, without a test suite they lost the ability to make sure that changes to their code base worked as expected. Without a test suite they could not ensure that changes to one part of their system did not break other parts of their system. So their defect rate began to rise. As the number of unintended defects rose, they started to fear making changes. They stopped cleaning their production code because they feared the changes would do more harm than good. Their production code began to rot. In the end they were left with no tests, tangled and bug-riddled production code, frustrated customers, and the feeling that their testing effort had failed them.

In a way they were right. Their testing effort had failed them. But it was their decision to allow the tests to be messy that was the seed of that failure. Had they kept their tests clean, their testing effort would not have failed. I can say this with some certainty because I have participated in, and coached, many teams who have been successful with clean unit tests.

The moral of the story is simple: Test code is just as important as production code. It is not a second-class citizen. It requires thought, design, and care. It must be kept as clean as production code.

### **Tests Enable the -ilities**

If you don't keep your tests clean, you will lose them. And without them, you lose the very thing that keeps your production code ﬂexible. Yes, you read that correctly. It is unit tests that keep our code ﬂexible, maintainable, and reusable. The reason is simple. If you have tests, you do not fear making changes to the code! Without tests every change is a possible bug. No matter how ﬂexible your architecture is, no matter how nicely partitioned your design, without tests you will be reluctant to make changes because of the fear that you will introduce undetected bugs.

But with tests that fear virtually disappears. The higher your test coverage, the less your fear. You can make changes with near impunity to code that has a less than stellar architecture and a tangled and opaque design. Indeed, you can improve that architecture and design without fear!

So having an automated suite of unit tests that cover the production code is the key to keeping your design and architecture as clean as possible. Tests enable all the -ilities, because tests enable change.

### **Clean Tests**

What makes a clean test? Three things. Readability, readability, and readability. Readability is perhaps even more important in unit tests than it is in production code. What makes tests readable? The same thing that makes all code readable: clarity, simplicity, and density of expression. In a test you want to say a lot with as few expressions as possible.

#### **One Assert per Test**

There is a school of thought4 that says that every test function in a JUnit test should have one and only one assert statement. This rule may seem draconian, but the advantage can be seen. Those tests come to a single conclusion that is quick and easy to understand.

I think the single assert rule is a good guideline. But I am not afraid to put more than one assert in a test. I think the best thing we can say is that the number of asserts in a test ought to be minimized.

#### **Single Concept per Test**

Perhaps a better rule is that we want to test a single concept in each test function. We don't want long test functions that go testing one miscellaneous thing after another.

**The best rule is that you should minimize the number of asserts per concept and test just one concept per test function.**

#### **F.I.R.S.T.**

Clean tests follow ﬁve other rules that form the above acronym

**Fast** Tests should be fast. They should run quickly. When tests run slow, you won't want to run them frequently. If you don't run them frequently, you won't ﬁnd problems early enough to ﬁx them easily. You won't feel as free to clean up the code. Eventually the code will begin to rot.

**Independent** Tests should not depend on each other. One test should not set up the conditions for the next test. You should be able to run each test independently and run the tests in any order you like. When tests depend on each other, then the ﬁrst one to fail causes a cascade of downstream failures, making diagnosis difﬁcult and hiding downstream defects.

**Repeatable** Tests should be repeatable in any environment. You should be able to run the tests in the production environment, in the QA environment, and on your laptop while riding home on the train without a network. If your tests aren't repeatable in any environment, then you'll always have an excuse for why they fail. You'll also ﬁnd yourself unable to run the tests when the environment isn't available.

**Self-Validating** The tests should have a Boolean output. Either they pass or fail. You should not have to read through a log ﬁle to tell whether the tests pass. You should not have to manually compare two different text ﬁles to see whether the tests pass. If the tests aren't self-validating, then failure can become subjective and running the tests can require a long manual evaluation.

**Timely** The tests need to be written in a timely fashion. Unit tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may ﬁnd the production code to be hard to test. You may decide that some production code is too hard to test. You may not design the production code to be testable.

## **Conclusion**

We have barely scratched the surface of this topic. Indeed, I think an entire book could be written about clean tests. Tests are as important to the health of a project as the production code is. Perhaps they are even more important, because tests preserve and enhance the ﬂexibility, maintainability, and reusability of the production code. So keep your tests constantly clean. Work to make them expressive and succinct. Invent testing APIs that act as domain-speciﬁc language that helps you write the tests. If you let the tests rot, then your code will rot too. Keep your tests clean.

## Chapter 10꞉ Class

### Classes Should Be Small!

<aside>
💡 **The ﬁrst rule of classes is that they should be small. The second rule of classes is that they should be smaller than that.**

</aside>

No, we're not going to repeat the exact same text from the Functions chapter. But as with functions, smaller is the primary rule when it comes to designing classes.

With functions we measured size by counting physical lines. With classes we use a different measure. We count responsibilities

The name of a class should describe what responsibilities it fulﬁlls. In fact, naming is probably the ﬁrst way of helping determine class size. If we cannot derive a concise name for a class, then it's likely too large. The more ambiguous the class name, the more likely it has too many responsibilities. For example, class names including weasel words like Processor or Manager or Super often hint at unfortunate aggregation of responsibilities.

### The Single Responsibility Principle

The Single Responsibility Principle (SRP)2 states that a class or module should have one, and only one, reason to change. This principle gives us both a deﬁnition of responsibility, and a guidelines for class size. Classes should have one responsibility—one reason to change.

Getting software to work and making software clean are two very different activities. Most of us have limited room in our heads, so we focus on getting our code to work more than organization and cleanliness. This is wholly appropriate. Maintaining a separation of concerns is just as important in our programming activities as it is in our programs.

The problem is that too many of us think that we are done once the program works. We fail to switch to the other concern of organization and cleanliness. We move on to the next problem rather than going back and breaking the overstuffed classes into decoupled units with single responsibilities.

Every sizable system will contain a large amount of logic and complexity. The primary goal in managing such complexity is to organize it so that a developer knows whereto look to ﬁnd things and need only understand the directly affected complexity at any given time. In contrast, a system with larger, multipurpose classes always hampers us by insisting we wade through lots of things we don't need to know right now

<aside>
💡 **To restate the former points for emphasis: We want our systems to be composed of many small classes, not a few large ones. Each small class encapsulates a single responsibility, has a single reason to change, and collaborates with a few others to achieve the desired system behaviors.**

</aside>

### Cohesion

Classes should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables. In general the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.

In general it is neither advisable nor possible to create such maximally cohesive classes; on the other hand, we would like cohesion to be high. When cohesion is high, it means that the methods and variables of the class are co-dependent and hang together as a logical whole.

The strategy of keeping functions small and keeping parameter lists short can sometimes lead to a proliferation of instance variables that are used by a subset of methods. When this happens, it almost always means that there is at least one other class trying to get out of the larger class. You should try to separate the variables and methods into two or more classes such that the new classes are more cohesive.

## Chapter 11꞉ Systems

### **How Would You Build a City?**

Could you manage all the details yourself? Probably not. Even managing an existing city is too much for one person. Yet, cities work (most of the time). They work because cities have teams of people who manage particular parts of the city, the water systems, power systems, trafﬁc, law enforcement, building codes, and so forth. Some of those people are responsible for the big picture, while others focus on the details.

Cities also work because they have evolved appropriate levels of abstraction and modularity that make it possible for individuals and the "components" they manage to work effectively, even without understanding the big picture.

Although software teams are often organized like that too, the systems they work on often don't have the same separation of concerns and levels of abstraction. Clean code helps us achieve this at the lower levels of abstraction.

### **Separate Constructing a System from Using It**

<aside>
💡 Software systems should separate the startup process, when the application objects are constructed and the dependencies are "wired" together, from the runtime logic that takes over after startup.

</aside>

The startup process is a concern that any application must address. It is the ﬁrst concern that we will examine in this chapter. The separation of concerns is one of the oldest and most important design techniques in our craft.

### **Dependency Injection**

A powerful mechanism for separating construction from use is Dependency Injection (DI), the application of Inversion of Control (IoC) to dependency management.3 Inversion of Control moves secondary responsibilities from an object to other objects that are dedicated to the purpose, thereby supporting the Single Responsibility Principle. In the context of dependency management, an object should not take responsibility for instantiating dependencies itself. Instead, it should pass this responsibility to another "authoritative" mechanism, thereby inverting the control. Because setup is a global concern, this authoritative mechanism will usually be either the "main" routine or a special-purpose container.

### **Scaling Up**

Cities grow from towns, which grow from settlements. At ﬁrst the roads are narrow and practically nonexistent, then they are paved, then widened over time. Small buildings and empty plots are ﬁlled with larger buildings, some of which will eventually be replaced with skyscrapers.

At ﬁrst there are no services like power, water, sewage, and the Internet (gasp!). These services are also added as the population and building densities increase. This growth is not without pain.

How many times have you driven, bumper to bumper through a road "improvement" project and asked yourself, "Why didn't they build it wide enough the ﬁrst time!?"

But it couldn't have happened any other way. Who can justify the expense of a sixlane highway through the middle of a small town that anticipates growth? Who would want such a road through their town?

It is a myth that we can get systems "right the ﬁrst time." Instead, we should implement only today's stories, then refactor and expand the system to implement new stories tomorrow. This is the essence of iterative and incremental agility. Test-driven development, refactoring, and the clean code they produce make this work at the code level.

<aside>
💡 Software systems are unique compared to physical systems. Their architectures can grow incrementally, if we maintain the proper separation of concerns.

</aside>

## **Optimize Decision Making**

Modularity and separation of concerns make decentralized management and decision making possible. In a sufﬁciently large system, whether it is a city or a software project, no one person can make all the decisions.

We all know it is best to give responsibilities to the most qualiﬁed persons. We often forget that it is also best to postpone decisions until the last possible moment. This isn't lazy or irresponsible; it lets us make informed choices with the best possible information. A premature decision is a decision made with suboptimal knowledge. We will have that much less customer feedback, mental reﬂection on the project, and experience with our implementation choices if we decide too soon.

## **Conclusion**

Systems must be clean too. An invasive architecture overwhelms the domain logic and impacts agility. When the domain logic is obscured, quality suffers because bugs ﬁnd it easier to hide and stories become harder to implement. If agility is compromised, productivity suffers and the beneﬁts of TDD are lost.

<aside>
💡 Whether you are designing systems or individual modules, never forget to use the simplest thing that can possibly work.

</aside>

## Chapter 12 : Emergence

What if there were four simple rules that you could follow that would help you create good designs as you worked? What if by following these rules you gained insights into the structure and design of your code, making it easier to apply principles such as SRP and DIP? What if these four rules facilitated the emergence of good designs

Many of us feel that Kent Beck's four rules of Simple Design1 are of signiﬁcant help in creating well-designed software.

According to Kent, a design is "simple" if it follows these rules:

- Runs all the tests
- Contains no duplication
- Expresses the intent of the programmer
- Minimizes the number of classes and methods

### **Simple Design Rule 1: Runs All the Tests**

First and foremost, a design must produce a system that acts as intended. A system might have a perfect design on paper, but if there is no simple way to verify that the system actually works as intended, then all the paper effort is questionable.

A system that is comprehensively tested and passes all of its tests all of the time is a testable system. That's an obvious statement, but an important one. Systems that aren't testable aren't veriﬁable. Arguably, a system that cannot be veriﬁed should never be deployed.

Fortunately, making our systems testable pushes us toward a design where our classes are small and single purpose. It's just easier to test classes that conform to the SRP. The more tests we write, the more we'll continue to push toward things that are simpler to test. So making sure our system is fully testable helps us create better designs.

Tight coupling makes it difﬁcult to write tests. So, similarly, the more tests we write, the more we use principles like DIP and tools like dependency injection, interfaces, and abstraction to minimize coupling. Our designs improve even more.

Remarkably, following a simple and obvious rule that says we need to have tests and run them continuously impacts our system's adherence to the primary OO goals of low coupling and high cohesion. Writing tests leads to better designs.

### **Simple Design Rules 2–4: Refactoring**

Once we have tests, we are empowered to keep our code and classes clean. We do this by incrementally refactoring the code. For each few lines of code we add, we pause and reﬂect on the new design. Did we just degrade it? If so, we clean it up and run our tests to demonstrate that we haven't broken anything. The fact that we have these tests eliminates the fear that cleaning up the code will break it!

During this refactoring step, we can apply anything from the entire body of knowledge about good software design. We can increase cohesion, decrease coupling, separate concerns, modularize system concerns, shrink our functions and classes, choose better names, and so on. This is also where we apply the ﬁnal three rules of simple design: Eliminate duplication, ensure expressiveness, and minimize the number of classes and methods.

### **No Duplication**

Duplication is the primary enemy of a well-designed system. It represents additional work, additional risk, and additional unnecessary complexity. Duplication manifests itself in many forms. Lines of code that look exactly alike are, of course, duplication. Lines of code that are similar can often be massaged to look even more alike so that they can be more easily refactored. And duplication can exist in other forms such as duplication of implementation.

### **Expressive**

Most of us have had the experience of working on convoluted code. Many of us have produced some convoluted code ourselves. It's easy to write code that we understand, because at the time we write it we're deep in an understanding of the problem we're trying to solve. Other maintainers of the code aren't going to have so deep an understanding.

The majority of the cost of a software project is in long-term maintenance. In order to minimize the potential for defects as we introduce change, it's critical for us to be able to understand what a system does. As systems become more complex, they take more and more time for a developer to understand, and there is an ever greater opportunity for a misunderstanding. Therefore, code should clearly express the intent of its author. The clearer the author can make the code, the less time others will have to spend understanding it. This will reduce defects and shrink the cost of maintenance.

You can express yourself by choosing good names. We want to be able to hear a class or function name and not be surprised when we discover its responsibilities.

You can also express yourself by keeping your functions and classes small. Small classes and functions are usually easy to name, easy to write, and easy to understand.

<aside>
💡 But the most important way to be expressive is to try. All too often we get our code working and then move on to the next problem without giving sufﬁcient thought to making that code easy for the next person to read. Remember, the most likely next person to read the code will be you. So take a little pride in your workmanship. Spend a little time with each of your functions and classes. Choose better names, split large functions into smaller functions, and generally just take care of what you've created. Care is a precious resource

</aside>

### **Minimal Classes and Methods**

Even concepts as fundamental as elimination of duplication, code expressiveness, and the SRP can be taken too far. In an effort to make our classes and methods small, we might create too many tiny classes and methods. So this rule suggests that we also keep our function and class counts low.

<aside>
💡 Our goal is to keep our overall system small while we are also keeping our functions and classes small. Remember, however, that this rule is the lowest priority of the four rules of Simple important to have tests, eliminate duplication, and express Design. So, although it's important to keep class and function count low, it's more important to have tests , eliminate duplication, and express yourself.

</aside>

### **Conclusion**

Is there a set of simple practices that can replace experience? Clearly not. On the other hand, the practices described in this chapter and in this book are a crystallized form of the many decades of experience enjoyed by the authors. Following the practice of simple design can and does encourage and enable developers to adhere to good principles and patterns that otherwise take years to learn.

## Chapter 13 Concurrency

Writing clean concurrent programs is hard—very hard. It is much easier to write code that executes in a single thread. It is also easy to write multithreaded code that looks fine on the surface but is broken at a deeper level. Such code works fine until the system is placed under stress.

### **Overview**

Concurrency is a decoupling strategy. It helps us decouple what gets done from when it gets done. In single-threaded applications what and when are so strongly coupled that the state of the entire application can often be determined by looking at the stack backtrace. A programmer who debugs such a system can set a breakpoint, or a sequence of breakpoints, and know the state of the system by which breakpoints are hit.

Decoupling what from when can dramatically improve both the throughput and structures of an application. From a structural point of view the application looks like many little collaborating computers rather than one big main loop. This can make the system easier to understand and offers some powerful ways to separate concerns.

consider a system that handles one user at a time and requires only one second of time per user. This system is fairly responsive for a few users, but as the number of users increases, the system's response time increases. No user wants to get in line behind 150 others! We could improve the response time of this system by handling many users concurrently.

consider a system that interprets large data sets but can only give a complete solution after processing all of them. Perhaps each data set could be processed on a different computer, so that many data sets are being processed in parallel.

### **Myths and Misconceptions**

- **Concurrency always improves performance.** Concurrency can sometimes improve performance, but only when there is a lot of wait time that can be shared between multiple threads or multiple processors. Neither situation is trivial.
- **Design does not change when writing concurrent programs.** In fact, the design of a concurrent algorithm can be remarkably different from the design of a single-threaded system. The decoupling of what from when usually has a huge effect on the structure of the system.
- **Correct concurrency is complex**, even for simple problems.

### **Conclusion Concurrent code**

Concurrent code is difficult to get right. Code that is simple to follow can become nightmarish when multiple threads and shared data get into the mix. If you are faced with writing concurrent code, you need to write clean code with rigor or else face subtle and infrequent failures
```

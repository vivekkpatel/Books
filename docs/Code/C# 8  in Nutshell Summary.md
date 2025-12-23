---
title: C# 8  in Nutshell Summary
---

# C# 8.0 in a Nutshell: The Definitive Reference by Joseph Albahari

# C# in Nutshell Summary

Go Through All Chapter Again for Use Full Content

### New things

- Linked List
- Queue
- Stack
- Lamda Expression
- Out Modifier
- Collections
- Progressive Linq quary with into keyword
- Conditional Compilation
- Catching Filesystem Events
- Generating Key for Strong Naming
- Regular Expression# Chapter 1 : Introduction

---

# **What’s New in C# 8.0**

### Indices and ranges

Indices let you refer to elements relative to the end of an array by using the ^ opera‐
tor. ^1 refers to the last element, ^2 refers to the second-to-last element, and so on:

```csharp
char[] vowels = new char[] {'a','e','i','o','u'};
char lastElement = vowels [^1]; // 'u'
char secondToLast = vowels [^2]; // 'o'
```

Ranges let you “slice” an array by using the .. operator:

```csharp
char[] firstTwo = vowels [..2]; // 'a', 'e'
char[] lastThree = vowels [2..]; // 'i', 'o', 'u'
char[] middleOne = vowels [2..3] // 'i'
char[] lastTwo = vowels [^2..]; // 'o', 'u'
```

C# implements indexes and ranges with the help of the Index and Range types

```csharp
Index last = ^1;
Range firstTwoRange = 0..2;
char[] firstTwo = vowels [firstTwoRange]; // 'a', 'e'
```

```csharp
//Indexer Inside Class
class Sentence
{
 string[] words = "The quick brown fox".Split();
 public string this [Index index] => words [index];
 public string[] this [Range range] => words [range];
}
```

### Null-coalescing assignment

```csharp
//The ??= operator assigns a variable only if it’s null. Instead of this:
if (s == null) s = "Hello, world";

//you can now write this:
s ??= "Hello, world";
```

### Using declarations

If you omit the brackets and statement block following a using statement, it
becomes a using declaration. The resource is then disposed when execution falls out‐
side the enclosing statement block:

```csharp
if (File.Exists ("file.txt"))
{
 using var reader = File.OpenText ("file.txt");
 Console.WriteLine (reader.ReadLine());
 ...
}
```

In this case, reader will be disposed when execution falls outside the if statement
block.

### Readonly members

C# 8 lets you apply the readonly modifier to a struct’s functions, ensuring that if the
function attempts to modify any field, a compile-time error is generated:

```csharp
struct Point
{
 public int X, Y;
 public readonly void ResetX() => X = 0; // Error!
}
```

If a readonly function calls a non-readonly function, the compiler generates a
warning (and defensively copies the struct to avoid the possibility of a mutation).

### Static local methods

Adding the `static` modifier to a local method prevents it from seeing the local variables and parameters of the enclosing method. This helps to reduce coupling as well as enabling the local method to declare variables as it pleases, without risk of colliding with those in the containing method.

### Default interface members

C# 8 lets you add a default implementation to an interface member, making it optional to implement:

```csharp
interface ILogger
{
 void Log (string text) => Console.WriteLine (text);
}
```

### Switch expressions

```csharp
string cardName = cardNumber switch // assuming cardNumber is an int
{
 13 => "King",
 12 => "Queen",
 11 => "Jack",
 _ => "Pip card" // equivalent to 'default'
};
```

### Tuple, positional, and property patterns

```csharp
int cardNumber = 12; string suite = "spades";
string cardName = (cardNumber, suite) switch
{
 (13, "spades") => "King of spades",
 (13, "clubs") => "King of clubs",
 ...
};
```

### Nullable reference types

```csharp
#nullable enable // Enable nullable reference types from this point on
string s1 = null; // Generates a compiler warning! (s1 is non-nullable)
string? s2 = null; // OK: s2 is nullable reference type
```

### Asynchronous streams

```csharp
async IAsyncEnumerable<int> RangeAsync (
 int start, int count, int delay)
{
 for (int i = start; i < start + count; i++)
 {
 await Task.Delay (delay);
 yield return i;
 }
}
```

---

# **What’s New in C# 7.0**

### **Numeric literal improvements**

Numeric literals in C# 7 can include underscores to improve readability. These are called digit separators and are ignored by the compiler:

int million = 1_000_000;

### **Patterns**

You can also introduce variables on the fly with the is operator. These are called pattern variables

```csharp
void Foo (object x) 
{ if (x is string s) 
  Console.WriteLine (s.Length); } 
The switch statement also supports patterns, so you can switch on type as well as constants
switch (x) 
{ case int i:
 Console.WriteLine ("It's an int!");
 break;
 case string s:  
Console.WriteLine (s.Length);  // We can use the s variable  
break; 
case bool b when b == true:    // Matches only when b is true  
Console.WriteLine ("True");  
 break; 
case null:  
Console.WriteLine ("Nothing");  
break; }
```

### **Local methods**

A local method is a method declared inside another function

```csharp
void WriteCubes()
 { Console.WriteLine (Cube (3));
  Console.WriteLine (Cube (4)); 
  Console.WriteLine (Cube (5));
  int Cube (int value) => value * value * value; }
```

### Deconstructors

```csharp
public void Deconstruct (out string firstName, out string lastName)
{
 int spacePos = name.IndexOf (' ');
 firstName = name.Substring (0, spacePos);
 lastName = name.Substring (spacePos + 1);
}
var joe = new Person ("Joe Bloggs");
var (first, last) = joe; // Deconstruction
Console.WriteLine (first); // Joe
Console.WriteLine (last); // Bloggs
```

### Tuples

```csharp
var bob = ("Bob", 23);
Console.WriteLine (bob.Item1); // Bob
Console.WriteLine (bob.Item2); // 23
```

---

# **What’s New in C# 6.0**

**Property initializers**

Property initializers : let you assign an initial value to an automatic property:

```csharp
public DateTime TimeCreated { get; set; } =DateTime.Now;
```

Initialized properties can also be read-only:

```csharp
public DateTime TimeCreated { get; } = DateTime.Now;
```

**String interpolation** 

offers a succinct alternative to string.Format:

```csharp
string s = $"It is {DateTime.Now.DayOfWeek} today"
```

**Namespaces**

A namespace is a domain for type names. Types are typically organized into hierarchical namespaces, making them easier to find and avoiding conflicts

- Namespaces are independent of assemblies, which are units of deployment such as an .exe or .dll
- Namespaces also have no impact on member visibility—public, internal, private, and so on.

The namepace keyword defines a namespace for types within that block.

For example:

```csharp
namespace Outer.Middle.Inner 
{
 class Class1 {}  
class Class2 {} 
}
```

The dots in the namespace indicate a hierarchy of nested namespaces. The code that follows is semantically identical to the preceding example:

```csharp
namespace Outer 
{  
    namespace Middle 
     {    
        namespace Inner    
        {     
         class Class1 {}      
        class Class2 {}    
        } 
     }
 }
```

---

## Writing a Class Versus an Interface

As a guideline:

• Use classes and subclasses for types that naturally share an implementation.

• Use interfaces for types that have independent implementations.

### Consider the following classes

```csharp
abstract class Animal {} 
abstract class Bird      : Animal {}
abstract class Insect     : Animal {} 
abstract class FlyingCreature : Animal {} 
abstract class Carnivore   : Animal {}

// Concrete classes:
class Ostrich : Bird {}
class Eagle  : Bird, FlyingCreature, Carnivore {} // Illegal 
class Bee   : Insect, FlyingCreature {}      // Illegal 
class Flea  : Insect, Carnivore {}        // Illegal

// The Eagle, Bee, and Flea classes do not compile because inheriting from multiple classes is prohibited. To resolve this, we must convert some of the types to interfaces. The question then arises, which types? Following our general rule, we could say that insects share an implementation, and birds share an implementation, so they remain classes. In contrast, flying creatures have independent mechanisms for flying, and carnivores have independent strategies for eating animals, so we would convert FlyingCreature and Carnivore to interfaces
interface IFlyingCreature {}
interface ICarnivore   {}
```

In a typical scenario, Bird and Insect might correspond to a Windows control and a web control; FlyingCreature and Carnivore might correspond to IPrintable and IUndoable.

---

## Enums

An enum is a special value type that lets you specify a group of named numeric constants. For example :

```csharp
public enum BorderSide { Left, Right, Top, Bottom }
```

- Each enum member has an underlying integral value. By default Underlying values are of type int.
- The constants 0, 1, 2... are automatically assigned, in the declaration order of
the enum members.

You can convert an enum instance to and from its underlying integral value with an
explicit cast:

```csharp
int i = (int) BorderSide.Left; 
BorderSide side = (BorderSide) i; 
bool leftOrRight = (int) side <= 2;
```

The numeric literal 0 is treated specially by the compiler in an enum expression and does not require an explicit cast:

`BorderSide b = 0; // No cast required if (b == 0) …`

There are two reasons for the special treatment of 0:

- The first member of an enum is often used as the “default” value.
- For combined enum types, 0 means “no flags.”# Chapter 10 : LINQ to XML

# Overview

NET Core provides a number of APIs for working with XML data. The primary choice for general-purpose XML document processing is LINQ to XML. LINQ to XML comprises a lightweight, LINQ-friendly XML document object model, plus a set of supplementary query operators.

<aside>
✏️ The LINQ to XML Document Object Model (DOM) is extremely well designed and highly performant. Even without LINQ, the LINQ to XML DOM is valuable as a lightweight
façade over the low-level XmlReader and XmlWriter classes

</aside>

All LINQ to XML types are defined in the `System.Xml.Linq` namespace.

# Architectural Overview

This section starts with a very brief introduction to the concept of a DOM, and then
explains the rationale behind LINQ to XML’s DOM.

### What Is a DOM?

```xml
<?xml version="1.0" encoding="utf-8"?>
<customer id="123" status="archived">
 <firstname>Joe</firstname>
 <lastname>Bloggs</lastname>
</customer>
```

As with all XML files, we start with a declaration and then a root element, whose name is customer. The customer element has two attributes, each with a name (id and status) and value ("123" and "archived"). Within customer, there are two child elements, firstname and lastname, each having simple text content ("Joe" and "Bloggs").

### The LINQ to XML DOM

LINQ to XML comprises two things

- An XML DOM, which we call the X-DOM
- A set of about 10 supplementary query operators

As you might expect, the X-DOM consists of types such as XDocument, XElement, and XAttribute. Interestingly, the X-DOM types are not tied to LINQ—you can load, instantiate, update, and save an X-DOM without ever writing a LINQ query

The distinguishing feature of the X-DOM is that it’s LINQ-friendly, meaning

- It has methods that emit useful IEnumerable sequences upon which you can query
- Its constructors are designed such that you can build an X-DOM tree through a
LINQ projection.

# X-DOM Overview

The most frequently used of these types is XElement. XObject is the root of the inheritance hierarchy; XElement and XDocument are roots of the containership hierarchy.

X-DOM tree created from the following code:

```csharp
string xml = @"<customer id='123' status='archived'>
 <firstname>Joe</firstname>
 <lastname>Bloggs<!--nice name--></lastname>
 </customer>";
XElement customer = XElement.Parse (xml);
```

XObject is the abstract base class for all XML content. It defines a link to the Parent element in the containership tree as well as an optional XDocument.

XNode is the base class for most XML content excluding attributes. The distinguishing feature of XNode is that it can sit in an ordered collection of mixed-type XNodes.

```xml
<data>
 Hello world
 <subelement1/>
 <!--comment-->
 <subelement2/>
</data>
```

Within the parent element , there’s first an XText node (Hello world), then an XElement node, then an XComment node, and then a second XElement node. In contrast, an XAttribute will tolerate only other XAttributes as peers.

Although an XNode can access its parent XElement, it has no concept of child nodes : this is the job of its subclass XContainer. XContainer defines members for dealing with children and is the abstract base class for XElement and XDocument.

### Loading and Parsing

```csharp
XDocument fromWeb = XDocument.Load ("http://albahari.com/sample.xml");
XElement fromFile = XElement.Load (@"e:\media\somefile.xml");
XElement config = XElement.Parse (
@"<configuration>
    <client enabled='true'>
        <timeout>30</timeout>
    </client>
</configuration>");
```

here’s how to manipulate the config element we just populated:

```csharp
foreach (XElement child in config.Elements())
 Console.WriteLine (child.Name); // client
XElement client = config.Element ("client");
bool enabled = (bool) client.Attribute ("enabled"); // Read attribute
Console.WriteLine (enabled); // True
client.Attribute ("enabled").SetValue (!enabled); // Update attribute
int timeout = (int) client.Element ("timeout"); // Read element
Console.WriteLine (timeout); // 30
client.Element ("timeout").SetValue (timeout * 2); // Update element
client.Add (new XElement ("retries", 3)); // Add new elememt
Console.WriteLine (config); // Implicitly call config.ToString()
//Here’s the result of that last Console.WriteLine:
<configuration>
 <client enabled="false">
 <timeout>60</timeout>
 <retries>3</retries>
 </client>
</configuration>
```

### Saving and Serializing

Calling `ToString` on any node converts its content to an XML string—formatted with line breaks and indentation as we just saw. (You can disable the line breaks and indentation by specifying `SaveOptions.DisableFormatting` when calling `ToString`.)

XElement and XDocument also provide a Save method that writes an X-DOM to a file, Stream, TextWriter, or XmlWriter. If you specify a file, an XML declaration is automatically written. There is also a WriteTo method defined in the XNode class, which accepts just an XmlWriter.

# Instantiating an X-DOM

Rather than using the Load or Parse methods, you can build an X-DOM tree by manually instantiating objects and adding them to a parent via XContainer’s Add method.

```csharp
//To construct an XElement and XAttribute, simply provide a name and value:
XElement lastName = new XElement ("lastname", "Bloggs");
lastName.Add (new XComment ("nice name"));
XElement customer = new XElement ("customer");
customer.Add (new XAttribute ("id", 123));
customer.Add (new XElement ("firstname", "Joe"));
customer.Add (lastName);
Console.WriteLine (customer.ToString());
//Here’s the result:
<customer id="123">
 <firstname>Joe</firstname>
 <lastname>Bloggs<!--nice name--></lastname>
</customer>
```

### Functional Construction

In our preceding example, it’s difficult to glean the XML structure from the code. XDOM supports another mode of instantiation, called functional construction

```csharp
XElement customer =
 new XElement ("customer", new XAttribute ("id", 123),
 new XElement ("firstname", "joe"),
 new XElement ("lastname", "bloggs",
 new XComment ("nice name")
 )
 );
```

This has two benefits. First, the code resembles the shape of the XML. Second, it can be incorporated into the select clause of a LINQ query.

```csharp
XElement query =
 new XElement ("customers",
 from c in dbContext.Customers.AsEnumerable()
 select
 new XElement ("customer", new XAttribute ("id", c.ID),
 new XElement ("firstname", c.FirstName),
 new XElement ("lastname", c.LastName,
 new XComment ("nice name")
 )
 )
 );
```

### Specifying Content

Functional construction is possible because the constructors for XElement (and XDocument) are overloaded to accept a params object array:

To see how, we need to examine how each content object is processed internally. Here are the decisions made by XContainer, in order:

- If the object is null, it’s ignored.
- If the object is based on XNode or XStreamingElement, it’s added as is to the Nodes collection.
- If the object is an XAttribute, it’s added to the Attributes collection.
- If the object is a string, it’s wrapped in an XText node and added to Nodes.
- If the object implements IEnumerable, it’s enumerated, and the same rules are applied to each element.
- Otherwise, the object is converted to a string, wrapped in an XText node, and then added to Nodes.

Everything ends up in one of two buckets: Nodes or Attributes. Furthermore, any object is valid content because it can always ultimately call ToString on it and treat it as an XText node.

### Automatic Deep Cloning

When a node or attribute is added to an element (whether via functional construction or an Add method), the node or attribute’s Parent property is set to that element. A node can have only one parent element: if you add an already parented node to a second parent, the node is automatically deep-cloned. I

```csharp
var address = new XElement ("address",
 new XElement ("street", "Lawley St"),
 new XElement ("town", "North Beach")
 );
var customer1 = new XElement ("customer1", address);
var customer2 = new XElement ("customer2", address);
customer1.Element ("address").Element ("street").Value = "Another St";
Console.WriteLine (
 customer2.Element ("address").Element ("street").Value); // Lawley St
```

# Navigating and Querying

### FirstNode, LastNode, and Nodes

FirstNode and LastNode give you direct access to the first or last child node; Nodes returns all children as a sequence. All three functions consider only direct descendants:

```csharp
var bench = new XElement ("bench",
 new XElement ("toolbox",
 new XElement ("handtool", "Hammer"),
 new XElement ("handtool", "Rasp")
 ),
 new XElement ("toolbox",
 new XElement ("handtool", "Saw"),
 new XElement ("powertool", "Nailgun")
 ),
 new XComment ("Be careful with the nailgun")
 );
foreach (XNode node in bench.Nodes())
 Console.WriteLine (node.ToString (SaveOptions.DisableFormatting) + ".");

//This is the output:
<toolbox><handtool>Hammer</handtool><handtool>Rasp</handtool></toolbox>.
<toolbox><handtool>Saw</handtool><powertool>Nailgun</powertool></toolbox>.
<!--Be careful with the nailgun-->.
```

### Retrieving elements

```csharp
foreach (XElement e in bench.Elements())
 Console.WriteLine (e.Name + "=" + e.Value); 
// toolbox=HammerRasp
 // toolbox=SawNailgun
```

```csharp
IEnumerable<string> query =
 from toolbox in bench.Elements()
 where toolbox.Elements().Any (tool => tool.Value == "Nailgun")
 select toolbox.Value;
RESULT: { "SawNailgun" }
```

```csharp
IEnumerable<string> query =
 from toolbox in bench.Elements()
 from tool in toolbox.Elements()
 where tool.Name == "handtool"
 select tool.Value;
RESULT: { "Hammer", "Rasp", "Saw" }
```

# Updating an X-DOM

### Simple Value Updates

```csharp
XElement settings = new XElement ("settings",
 new XElement ("timeout", 30)
 );
settings.SetValue ("blah");
Console.WriteLine (settings.ToString()); // <settings>blah</settings>
```

### Updating Child Nodes and Attributes

```csharp
XElement settings = new XElement ("settings");
settings.SetElementValue ("timeout", 30); // Adds child node
settings.SetElementValue ("timeout", 60); // Update it to 60
```

### Updating Through the Parent

```csharp
XElement items = new XElement ("items",
 new XElement ("one"),
 new XElement ("three")
 );
items.FirstNode.AddAfterSelf (new XElement ("two"));
Here’s the result:
<items><one /><two /><three /></items>
```

```csharp
XElement items = XElement.Parse ("<items><one/><two/><three/></items>");
items.FirstNode.ReplaceWith (new XComment ("One was here"));
Here’s the result:
<items><!--one was here--><two /><three /></items>
```

### Removing a sequence of nodes or attributes

```csharp
XElement contacts = XElement.Parse (
@"<contacts>
 <customer name='Mary'/>
 <customer name='Chris' archived='true'/>
 <supplier name='Susan'>
 <phone archived='true'>012345678<!--confidential--></phone>
 </supplier>
 </contacts>");
//The following removes all customers:
contacts.Elements ("customer").Remove();
//The following removes all archived contacts (so Chris disappears):
contacts.Elements().Where (e => (bool?) e.Attribute ("archived") == true)
 .Remove();
```

# Working with Values

### Setting Values

There are two ways to assign a value: call SetValue or assign the Value property.
SetValue is more flexible because it accepts not just strings, but other simple data
types, too

```csharp
var e = new XElement ("date", DateTime.Now);
e.SetValue (DateTime.Now.AddDays(1));
Console.Write (e.Value); // 2019-10-02T16:39:10.734375+09:00
```

### Getting Values

```csharp
XElement e = new XElement ("now", DateTime.Now);
DateTime dt = (DateTime) e;
XAttribute a = new XAttribute ("resolution", 1.234);
double res = (double) a;
```

### Automatic XText Concatenation

```csharp
var e1 = new XElement ("test", "Hello"); e1.Add ("World");
var e2 = new XElement ("test", "Hello", "World");
//If you specifically create XText nodes, however, you end up with multiple children:
var e = new XElement ("test", new XText ("Hello"), new XText ("World"));
Console.WriteLine (e.Value); // HelloWorld
Console.WriteLine (e.Nodes().Count()); // 2
```

# Documents and Declarations

### XDocument

The simplest valid XDocument has just a root element

```csharp
var doc = new XDocument (
 new XElement ("test", "data")
 );
```

The next example produces a simple but correct XHTML file, illustrating all the
constructs that an XDocument can accept:

```csharp
var styleInstruction = new XProcessingInstruction (
 "xml-stylesheet", "href='styles.css' type='text/css'");
var docType = new XDocumentType ("html",
 "-//W3C//DTD XHTML 1.0 Strict//EN",
 "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd", null);
XNamespace ns = "http://www.w3.org/1999/xhtml";
var root =
 new XElement (ns + "html",
 new XElement (ns + "head",
 new XElement (ns + "title", "An XHTML page")),
 new XElement (ns + "body",
 new XElement (ns + "p", "This is the content"))
 );
var doc =
 new XDocument (
 new XDeclaration ("1.0", "utf-8", "no"),
 new XComment ("Reference a stylesheet"),
 styleInstruction,
 docType,
 root);
doc.Save ("test.html");
```

The resultant test.html reads as follows:

```csharp
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!--Reference a stylesheet-->
<?xml-stylesheet href='styles.css' type='text/css'?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
 "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
 <head>
 <title>An XHTML page</title>
 </head>
 <body>
 <p>This is the content</p>
 </body>
</html>
```

### XML Declarations

```csharp
var doc =
    new XDocument (
        new XDeclaration ("1.0", "utf-16", "yes"),
        new XElement ("test", "data")
    );

string tempPath = Path.Combine (Path.GetTempPath(), "test.xml");
doc.Save (tempPath);
```

### Writing a declaration to a string

```csharp
var doc = new XDocument (
 new XDeclaration ("1.0", "utf-8", "yes"),
 new XElement ("test", "data")
 );
var output = new StringBuilder();
var settings = new XmlWriterSettings { Indent = true };
using (XmlWriter xw = XmlWriter.Create (output, settings))
 doc.Save (xw);
Console.WriteLine (output.ToString());
//This is the result:
<?xml version="1.0" encoding="utf-16" standalone="yes"?>
<test>data</test>
```

# Names and Namespaces

### Namespaces in XML

Suppose that we want to define a customer element in the namespace OReilly.Nutshell.CSharp. There are two ways to proceed. The first is to use the xmlns attribute:

`<customer xmlns="OReilly.Nutshell.CSharp"/>`

xmlns is a special reserved attribute. When used in this manner, it performs two functions

• It specifies a namespace for the element in question.
• It specifies a default namespace for all descendant elements.

This means that in the following example, address and postcode implicitly reside in the OReilly.Nutshell.CSharp namespace:

```xml
<customer xmlns="OReilly.Nutshell.CSharp">
 <address>
 <postcode>02138</postcode>
 </address>
</customer>
//If we want address and postcode to have no namespace, we’d need to do this:
<customer xmlns="OReilly.Nutshell.CSharp">
 <address xmlns="">
 <postcode>02138</postcode> <!-- postcode now inherits empty ns -->
 </address>
</customer>
```

### Prefixes

The other way to specify a namespace is with a prefix. A prefix is an alias that you assign to a namespace to save typing. There are two steps in using a prefix—defining the prefix and using it. You can do both together:

`<nut:customer xmlns:nut="OReilly.Nutshell.CSharp"/>`

```xml
<nut:customer xmlns:nut="OReilly.Nutshell.CSharp">
 <nut:firstname>Joe</firstname>
</customer>
```

### Attributes

You can assign namespaces to attributes, too. The main difference is that it always requires a prefix; for instance:

```xml
<customer xmlns:nut="OReilly.Nutshell.CSharp" nut:id="123" />
```

### Specifying Namespaces in the X-DOM

There are a couple of ways to specify an XML namespace. The first is to enclose it in braces, before the local name:

```csharp
var e = new XElement ("{http://domain.com/xmlspace}customer", "Bloggs");
Console.WriteLine (e.ToString());
//This yields the resulting XML:
<customer xmlns="http://domain.com/xmlspace">Bloggs</customer>
```

The second (and more performant) approach is to use the XNamespace and XName types.

```csharp
public sealed class XNamespace
{
 public string NamespaceName { get; }
}
public sealed class XName // A local name with optional namespace
{
 public string LocalName { get; }
 public XNamespace Namespace { get; } // Optional
}
```

Both types define implicit casts from string, so the following is legal

```csharp
XNamespace ns = "http://domain.com/xmlspace";
XName localName = "customer";
XName fullName = "{http://domain.com/xmlspace}customer";

XNamespace ns = "http://domain.com/xmlspace";
XName fullName = ns + "customer";
Console.WriteLine (fullName); // {http://domain.com/xmlspace}customer

//Specifying a namespace is the same whether for an element or an attribute:
XNamespace ns = "http://domain.com/xmlspace";
var data = new XElement (ns + "data",
 new XAttribute (ns + "id", 123)
 );
```

### The X-DOM and Default Namespaces

The X-DOM ignores the concept of default namespaces until it comes time to actually output XML. This means that when you construct a child XElement, you must give it a namespace explicitly if needed: it will not inherit from the parent:

```csharp
XNamespace ns = "http://domain.com/xmlspace";
var data = new XElement (ns + "data",
 new XElement (ns + "customer", "Bloggs"),
 new XElement (ns + "purchase", "Bicycle")
 );
```

The X-DOM does, however, apply default namespaces when reading and outputting
XML:

```csharp
Console.WriteLine (data.ToString());
OUTPUT:
 <data xmlns="http://domain.com/xmlspace">
 <customer>Bloggs</customer>
 <purchase>Bicycle</purchase>
 </data>
Console.WriteLine (data.Element (ns + "customer").ToString());
OUTPUT:
 <customer xmlns="http://domain.com/xmlspace">Bloggs</customer>
```

If you construct XElement children without specifying namespaces; in other words:

```csharp
XNamespace ns = "http://domain.com/xmlspace";
var data = new XElement (ns + "data",
 new XElement ("customer", "Bloggs"),
 new XElement ("purchase", "Bicycle")
 );
Console.WriteLine (data.ToString());
you get this result, instead:
<data xmlns="http://domain.com/xmlspace">
 <customer xmlns="">Bloggs</customer>
<purchase xmlns="">Bicycle</purchase>
</data>
```

Another trap is failing to include a namespace when navigating an X-DOM

```csharp
XNamespace ns = "http://domain.com/xmlspace";
var data = new XElement (ns + "data",
 new XElement (ns + "customer", "Bloggs"),
 new XElement (ns + "purchase", "Bicycle")
 );
XElement x = data.Element (ns + "customer"); // ok
XElement y = data.Element ("customer"); // null
```

### Prefixes

The X-DOM treats prefixes just as it treats namespaces: purely as a serialization function. This means that you can choose to completely ignore the issue of prefixes —and get by! The only reason you might want to do otherwise is for efficiency when outputting to an XML file.

```csharp
XNamespace ns1 = "http://domain.com/space1";
XNamespace ns2 = "http://domain.com/space2";
var mix = new XElement (ns1 + "data",
 new XElement (ns2 + "element", "value"),
 new XElement (ns2 + "element", "value"),
 new XElement (ns2 + "element", "value")
 );
//By default, XElement will serialize this, as follows:
<data xmlns="http://domain.com/space1">
 <element xmlns="http://domain.com/space2">value</element>
 <element xmlns="http://domain.com/space2">value</element>
 <element xmlns="http://domain.com/space2">value</element>
</data>
```

```csharp
XNamespace xsi = "http://www.w3.org/2001/XMLSchema-instance";
var nil = new XAttribute (xsi + "nil", true);
var cust = new XElement ("customers",
 new XAttribute (XNamespace.Xmlns + "xsi", xsi),
 new XElement ("customer",
 new XElement ("lastname", "Bloggs"),
 new XElement ("dob", nil),
 new XElement ("credit", nil)
 )
 );
This is its XML:
<customers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <customer>
 <lastname>Bloggs</lastname>
 <dob xsi:nil="true" />
 <credit xsi:nil="true" />
 </customer>
</customers>
```

# Annotations

You can attach custom data to any XObject with an annotation. Annotations are intended for your own private use and are treated as black boxes by X-DOM. If you’ve ever used the Tag property on a Windows Forms or WPF control, you’ll be familiar with the concept—the difference is that you have multiple annotations, and your annotations can be privately scoped. You can create an annotation that other types cannot even see—let alone overwrite

```csharp
//The following methods on XObject add and remove annotations:
public void AddAnnotation (object annotation)
public void RemoveAnnotations<T>() where T : class
//The following methods retrieve annotations:
public T Annotation<T>() where T : class
public IEnumerable<T> Annotations<T>() where T : class
```

```csharp
//Annotation with Custom Type
void Main()
{
    XElement e = new XElement ("test");
    
    e.AddAnnotation (new CustomData { Message = "Hello" } );
    e.Annotations<CustomData>().First().Message.Dump();
    
    e.RemoveAnnotations<CustomData>();
    e.Annotations<CustomData>().Count().Dump();	
}

class CustomData				// Private nested type
{
     internal string Message;
}
```

# Projecting into an X-DOM

```csharp
//suppose that we want to retrieve customers from a database into the following XML
<customers>
 <customer id="1">
 <name>Sue</name>
 <buys>3</buys>
 </customer>
 ...
</customers>
//We start by writing a functional construction expression for the X-DOM using simple literals:
var customers =
 new XElement ("customers",
 new XElement ("customer", new XAttribute ("id", 1),
 new XElement ("name", "Sue"),
 new XElement ("buys", 3)
 )
 );
//We then turn this into a projection and build a LINQ query around it:
var customers =
 new XElement ("customers",
 // We must call AsEnumerable() due to a bug in EF Core.
 from c in dbContext.Customers.AsEnumerable()
 select
 new XElement ("customer", new XAttribute ("id", c.ID),
 new XElement ("name", c.Name),
 new XElement ("buys", c.Purchases.Count)
 )
 );
```

Here’s the result:

```csharp
<customers>
 <customer id="1">
 <name>Tom</name>
 <buys>3</buys>
 </customer>
 <customer id="2">
 <name>Harry</name>
 <buys>2</buys>
 </customer>
 ...
</customers>
```# Chapter 11 : Other XML and JSON Technologies

# XmlReader

XmlReader is a high-performance class for reading an XML stream in a low-level, forward-only manner.

### Read Nodes

```csharp
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<customer id="123" status="archived">
 <firstname>Jim</firstname>
 <lastname>Bo</lastname>
</customer>
```

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.IgnoreWhitespace = true;
using XmlReader reader = XmlReader.Create ("customer.xml", settings);
while (reader.Read())
{
 Console.Write (new string (' ', reader.Depth * 2)); // Write indentation
 Console.Write (reader.NodeType.ToString());
 if (reader.NodeType == XmlNodeType.Element ||
 reader.NodeType == XmlNodeType.EndElement)
 {
 Console.Write (" Name=" + reader.Name);
 }
 else if (reader.NodeType == XmlNodeType.Text)
 {
 Console.Write (" Value=" + reader.Value);
 }
 Console.WriteLine ();
}
```

The output is as follows:

```csharp
XmlDeclaration
Element Name=customer
 Element Name=firstname
 Text Value=Jim
 EndElement Name=firstname
 Element Name=lastname
 Text Value=Bo
 EndElement Name=lastname
EndElement Name=customer
```

### Read Elements

```csharp
#load ".\Create sample files.linq"

XmlReaderSettings settings = new XmlReaderSettings();
settings.IgnoreWhitespace = true;

using XmlReader r = XmlReader.Create ("customerCredit.xml", settings);

r.MoveToContent();                // Skip over the XML declaration
r.ReadStartElement ("customer");
string firstName = r.ReadElementContentAsString ("firstname", "");
string lastName = r.ReadElementContentAsString ("lastname", "");
decimal creditLimit = r.ReadElementContentAsDecimal ("creditlimit", "");

r.MoveToContent();      // Skip over that pesky comment
r.ReadEndElement();     // Read the closing customer tag

$"{firstName} {lastName} credit limit: {creditLimit}".Dump();
```

### Reading Attributes

```csharp
//Given the XML fragment:
<customer id="123" status="archived"/>
//we could read its attributes as follows:
Console.WriteLine (reader ["id"]); // 123
Console.WriteLine (reader ["status"]); // archived
Console.WriteLine (reader ["bogus"] == null); // True
```

### Attribute nodes

```csharp
Returning to our previous example:
<customer id="123" status="archived"/>
we can do this:
reader.MoveToAttribute ("status");
string status = reader.ReadContentAsString();
reader.MoveToAttribute ("id");
int id = reader.ReadContentAsInt();
```

# XmlWriter

XmlWriter is a forward-only writer of an XML stream. The design of XmlWriter is symmetrical to XmlReader.

```csharp
XmlWriterSettings settings = new XmlWriterSettings();
settings.Indent = true;
using XmlWriter writer = XmlWriter.Create ("foo.xml", settings);
writer.WriteStartElement ("customer");
writer.WriteElementString ("firstname", "Jim");
writer.WriteElementString ("lastname", "Bo");
writer.WriteEndElement();
```

```csharp
<?xml version="1.0" encoding="utf-8"?>
<customer>
 <firstname>Jim</firstname>
 <lastname>Bo</lastname>
</customer>
```

Writing Attributes

```csharp
writer.WriteStartElement ("customer");
writer.WriteAttributeString ("id", "1");
writer.WriteAttributeString ("status", "archived");
```

Namespaces and Prefixes

```csharp
writer.WriteStartElement ("o", "customer", "http://oreilly.com");
writer.WriteElementString ("o", "firstname", "http://oreilly.com", "Jim");
writer.WriteElementString ("o", "lastname", "http://oreilly.com", "Bo");
writer.WriteEndElement();
//The output is now as follows:
<?xml version="1.0" encoding="utf-8"?>
<o:customer xmlns:o='http://oreilly.com'>
 <o:firstname>Jim</o:firstname>
 <o:lastname>Bo</o:lastname>
</o:customer>
```

# Patterns for Using XmlReader/XmlWriter

### Working with Hierarchical Data

Consider the following classes:

```csharp
public class Contacts
{
 public IList<Customer> Customers = new List<Customer>();
 public IList<Supplier> Suppliers = new List<Supplier>();
}
public class Customer { public string FirstName, LastName; }
public class Supplier { public string Name; }
```

Suppose that you want to use XmlReader and XmlWriter to serialize a Contacts object to XML, as in the following:

```csharp
<?xml version="1.0" encoding="utf-8"?>
<contacts>
 <customer id="1">
 <firstname>Jay</firstname>
 <lastname>Dee</lastname>
 </customer>
 <customer> <!-- we'll assume id is optional -->
 <firstname>Kay</firstname>
 <lastname>Gee</lastname>
 </customer>
 <supplier>
 <name>X Technologies Ltd</name>
 </supplier>
</contacts>
```

The best approach is not to write one big method, but to encapsulate XML func‐
tionality in the Customer and Supplier types themselves by writing ReadXml and
WriteXml methods on these types.

The pattern in doing so is straightforward:

- ReadXml and WriteXml leave the reader/writer at the same depth when they exit
- ReadXml reads the outer element, whereas WriteXml writes only its inner content.

Here’s how we would write the Customer type

```csharp
public class Customer
{
 public const string XmlName = "customer";
 public int? ID;
 public string FirstName, LastName;
 public Customer () { }
 public Customer (XmlReader r) { ReadXml (r); }
 public void ReadXml (XmlReader r)
 {
 if (r.MoveToAttribute ("id")) ID = r.ReadContentAsInt();
 r.ReadStartElement();
 FirstName = r.ReadElementContentAsString ("firstname", "");
 LastName = r.ReadElementContentAsString ("lastname", "");
 r.ReadEndElement();
 }
 public void WriteXml (XmlWriter w)
 {
 if (ID.HasValue) w.WriteAttributeString ("id", "", ID.ToString());
 w.WriteElementString ("firstname", FirstName);
 w.WriteElementString ("lastname", LastName);
 }
}
```

```csharp
The Supplier class is analogous:
public class Supplier
{
 public const string XmlName = "supplier";
 public string Name;
public Supplier () { }
 public Supplier (XmlReader r) { ReadXml (r); }
 public void ReadXml (XmlReader r)
 {
 r.ReadStartElement();
 Name = r.ReadElementContentAsString ("name", "");
 r.ReadEndElement();
 }
 public void WriteXml (XmlWriter w) =>
 w.WriteElementString ("name", Name);
}
```

the Contacts class, we must enumerate the customers element in ReadXml, checking whether each subelement is a customer or a supplier. We also need to code around the empty element trap:

```csharp
public void ReadXml (XmlReader r)
{
 bool isEmpty = r.IsEmptyElement; // This ensures we don't get
 r.ReadStartElement(); // snookered by an empty
 if (isEmpty) return; // <contacts/> element!
 while (r.NodeType == XmlNodeType.Element)
 {
 if (r.Name == Customer.XmlName) Customers.Add (new Customer (r));
 else if (r.Name == Supplier.XmlName) Suppliers.Add (new Supplier (r));
 else
 throw new XmlException ("Unexpected node: " + r.Name);
 }
 r.ReadEndElement();
}
public void WriteXml (XmlWriter w)
{
 foreach (Customer c in Customers)
 {
 w.WriteStartElement (Customer.XmlName);
 c.WriteXml (w);
 w.WriteEndElement();
 }
 foreach (Supplier s in Suppliers)
 {
 w.WriteStartElement (Supplier.XmlName);
 s.WriteXml (w);
 w.WriteEndElement();
 }
}
```

Here’s how to serialize a Contacts object populated with Customers and Suppliers to an XML file:

```csharp
var settings = new XmlWriterSettings();
settings.Indent = true; // To make visual inspection easier
using XmlWriter writer = XmlWriter.Create ("contacts.xml", settings);
var cts = new Contacts()
// Add Customers and Suppliers...
writer.WriteStartElement ("contacts");
cts.WriteXml (writer);
writer.WriteEndElement();
```

Here’s how to deserialize from the same file:

```csharp
var settings = new XmlReaderSettings();
settings.IgnoreWhitespace = true;
settings.IgnoreComments = true;
settings.IgnoreProcessingInstructions = true;
using XmlReader reader = XmlReader.Create("contacts.xml", settings);
reader.MoveToContent();
var cts = new Contacts();
cts.ReadXml(reader);
```

### Mixing XmlReader/XmlWriter with an X-DOM

For instance, suppose that we have an XML logfile structured as follows:

```csharp
<log>
 <logentry id="1">
 <date>...</date>
 <source>...</source>
 ...
 </logentry>
 ...
</log>
```

If there were a million logentry elements, reading the entire thing into an X-DOM would waste memory. A better solution is to traverse each logentry with an XmlReader and then use XElement to process the elements individually:

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.IgnoreWhitespace = true;
using XmlReader r = XmlReader.Create ("logfile.xml", settings);
r.ReadStartElement ("log");
while (r.Name == "logentry")
{
 XElement logEntry = (XElement) XNode.ReadFrom (r);
 int id = (int) logEntry.Attribute ("id");
 DateTime date = (DateTime) logEntry.Element ("date");
 string source = (string) logEntry.Element ("source");
 ...
}
r.ReadEndElement();
```

For instance, we could rewrite Customer’s ReadXml method as follows:

```csharp
public void ReadXml (XmlReader r)
{
 XElement x = (XElement) XNode.ReadFrom (r);
 ID = (int) x.Attribute ("id");
 FirstName = (string) x.Element ("firstname");
 LastName = (string) x.Element ("lastname");
}
```

### Using XmlWriter with XElement

```csharp
using XmlWriter w = XmlWriter.Create ("logfile.xml");
w.WriteStartElement ("log");
for (int i = 0; i < 1000000; i++)
{
 XElement e = new XElement ("logentry",
 new XAttribute ("id", i),
 new XElement ("date", DateTime.Today.AddDays (-1)),
 new XElement ("source", "test"));
 e.WriteTo (w);
}
w.WriteEndElement ();
```

# Working with JSON

JSON has become a popular alternative to XML. Although it lacks the advanced fea‐ tures of XML (such as namespaces, prefixes, and schemas), it benefits from being simple and uncluttered, with a format similar to what you would get from convert‐ ing a JavaScript object to a string.

In the past, you needed third-party libraries such as Json.NET to work with JSON in C#, but now you have the option of using .NET Core’s built-in classes. Compared to Json.NET, the built-in classes are less powerful, but simpler, faster, and more memory efficient.

### Json Reader

```csharp
{
 "FirstName":"Sara",
 "LastName":"Wells",
 "Age":35,
 "Friends":["Dylan","Ian"]
}
```

The following code parses the file by enumerating its JSON tokens. A token is the beginning or end of an object, the beginning or end of an array, the name of a property, or an array or property value (string, number, true, false, or null).

```csharp
byte[] data = File.ReadAllBytes ("people.json");
Utf8JsonReader reader = new Utf8JsonReader (data);
while (reader.Read())
{
switch (reader.TokenType)
 {
 case JsonTokenType.StartObject:
 Console.WriteLine ($"Start of object");
 break;
 case JsonTokenType.EndObject:
 Console.WriteLine ($"End of object");
 break;
 case JsonTokenType.StartArray:
 Console.WriteLine();
 Console.WriteLine ($"Start of array");
 break;
 case JsonTokenType.EndArray:
 Console.WriteLine ($"End of array");
 break;
 case JsonTokenType.PropertyName:
 Console.Write ($"Property: {reader.GetString()}");
 break;
 case JsonTokenType.String:
 Console.WriteLine ($" Value: {reader.GetString()}");
 break;
 case JsonTokenType.Number:
 Console.WriteLine ($" Value: {reader.GetInt32()}");
 break;
 default:
 Console.WriteLine ($"No support for {reader.TokenType}");
 break;
 }
}
```

Here’s the output:

```csharp
Start of object
Property: FirstName Value: Sara
Property: LastName Value: Wells
Property: Age Value: 35
Property: Friends
Start of array
 Value: Dylan
 Value: Ian
End of array
End of object
```

### JsonWriter

```csharp
var options = new JsonWriterOptions { Indented = true };
using (var stream = File.Create ("MyFile.json"))
using (var writer = new Utf8JsonWriter (stream, options))
{
 writer.WriteStartObject();
 // Property name and value specified in one call
 writer.WriteString ("FirstName", "Dylan");
writer.WriteString ("LastName", "Lockwood");
 // Property name and value specified in separate calls
 writer.WritePropertyName ("Age");
 writer.WriteNumberValue (46);
 writer.WriteCommentValue ("This is a (non-standard) comment");
 writer.WriteEndObject();
}
```

```csharp
{
 "FirstName": "Dylan",
 "LastName": "Lockwood",
 "Age": 46
 /*This is a (non-standard) comment*/
}
```

### JsonDocument

System.Text.Json.JsonDocument parses JSON data into a read-only DOM com‐ posed of lazily populated JsonElement instances that you can access randomly

<aside>
👉🏿 JsonDocument is fast and efficient, employing pooled memory to minimize garbage collection. This means that you must dis‐ pose the JsonDocument after use; otherwise, its memory will not be returned to the pool.

</aside>

```csharp
using JsonDocument document = JsonDocument.Parse ("123");
JsonElement root = document.RootElement;
Console.WriteLine (root.ValueKind); // Number
```

Reading simple values

```csharp
using JsonDocument document = JsonDocument.Parse ("123");
int number = document.RootElement.GetInt32();
```

Reading JSON arrays

```csharp
using JsonDocument document = JsonDocument.Parse (@"[1, 2, 3, 4, 5]");
int length = document.RootElement.GetArrayLength(); // 5
int value = document.RootElement[3].GetInt32(); // 4
```

Reading JSON objects

```csharp
using JsonDocument document = JsonDocument.Parse (@"{ ""Age"": 32}");
JsonElement root = document.RootElement;
int age = root.GetProperty ("Age").GetInt32();
```

Here’s how we could “discover” the Age property

```csharp
JsonProperty ageProp = root.EnumerateObject().First();
string name = ageProp.Name; // Age
JsonElement value = ageProp.Value;
Console.WriteLine (value.ValueKind); // Number
Console.WriteLine (value.GetInt32()); // 32
```

### JsonDocument and LINQ

JsonDocument lends itself well to LINQ. Given the following JSON file

```csharp
[
 {
 "FirstName":"Sara",
 "LastName":"Wells",
 "Age":35,
 "Friends":["Ian"]
 },
 {
 "FirstName":"Ian",
 "LastName":"Weems",
 "Age":42,
 "Friends":["Joe","Eric","Li"]
 },
 {
 "FirstName":"Dylan",
 "LastName":"Lockwood",
 "Age":46,
 "Friends":["Sara","Ian"]
 }
]
```

we can use JsonDocument to query this with LINQ, as follows

```csharp
using var stream = File.OpenRead (jsonPath);
using JsonDocument document = JsonDocument.Parse (json);
var query =
 from person in document.RootElement.EnumerateArray()
 select new
 {
 FirstName = person.GetProperty ("FirstName").GetString(),
 Age = person.GetProperty ("Age").GetInt32(),
 Friends =
 from friend in person.GetProperty ("Friends").EnumerateArray()
 select friend.GetString()
 };
```

### Making updates with a JSON writer

```csharp
using var json = File.OpenRead (jsonPath);
using JsonDocument document = JsonDocument.Parse (json);
var options = new JsonWriterOptions { Indented = true };
using (var outputStream = File.Create ("NewFile.json"))
using (var writer = new Utf8JsonWriter (outputStream, options))
{
 writer.WriteStartArray();
 foreach (var person in document.RootElement.EnumerateArray())
 {
 int friendCount = person.GetProperty ("Friends").GetArrayLength();
 if (friendCount >= 2)
 person.WriteTo (writer);
 }
}
```# Chapter 12 : Disposal and Garbage Collection

# Overview

Some objects require explicit tear-down code to release resources such as open files, locks, operating system handles, and unmanaged objects. In .NET parlance, this is called disposal, and it is supported through the `IDisposable` interface. The managed memory occupied by unused objects must also be reclaimed at some point; this function is known as garbage collection and is performed by the CLR.

Disposal differs from garbage collection in that disposal is usually explicitly instiga‐ ted; garbage collection is totally automatic. In other words, the programmer takes care of such things as releasing file handles, locks, and operating system resources while the CLR takes care of releasing memory.

This chapter discusses both disposal and garbage collection, also describing C# finalizers and the pattern by which they can provide a backup for disposal.

# IDisposable, Dispose, and Close

The .NET Core defines a special interface for types requiring a tear-down method

```csharp
public interface IDisposable
{
 void Dispose();
}
```

C#’s using statement provides a syntactic shortcut for calling Dispose on objects that implement IDisposable, using a try/finally block:

```csharp
using (FileStream fs = new FileStream ("myFile.txt", FileMode.Open))
{
 // ... Write to the file ...
}
```

The compiler converts this to the following

```csharp
FileStream fs = new FileStream ("myFile.txt", FileMode.Open);
try
{
 // ... Write to the file ...
}
finally
{
 if (fs != null) ((IDisposable)fs).Dispose();
}
```

In simple scenarios, writing your own disposable type is just a matter of implementing IDisposable and writing the Dispose method:

```csharp
sealed class Demo : IDisposable
{
 public void Dispose()
 {
 // Perform cleanup / tear-down.
 ...
 }
}
```

### Standard Disposal Semantics

.NET Core follows a de facto set of rules in its disposal logic. These rules are not hard-wired to .NET Core or the C# language in any way; their purpose is to define a consistent protocol to consumers. Here they are:

1. .NET Core follows a de facto set of rules in its disposal logic. These rules are not hard-wired to .NET Core or the C# language in any way; their purpose is to define a consistent protocol to consumers. Here they are:
2. Calling an object’s Dispose method repeatedly causes no error.
3. If disposable object x “owns” disposable object y, x’s Dispose method automati‐ cally calls y’s Dispose method—unless instructed otherwise.

According to rule 3, a container object automatically disposes its child objects. A good example is a **Windows Forms container control such as a Form or Panel. The container can host many child controls, yet you don’t dispose every one of them explicitly;** closing or disposing the parent control or form takes care of the whole lot.

### Close and Stop

Some types define a method called Close in addition to Dispose. The Framework is not completely consistent on the semantics of a Close method, although in nearly all cases it’s either of the following:

- Functionally identical to Dispose
- A functional subset of Dispose

An example of the latter is IDbConnection: a Closed connection can be re-Opened; a Disposed connection cannot. Another example is a Windows Form activated with ShowDialog: Close hides it; Dispose releases its resources.

### When to Dispose

A safe rule to follow (in nearly all cases) is “if in doubt, dispose.” Objects wrapping an unmanaged resource handle will nearly always require disposal in order to free the handle. Examples include file or network streams, network sockets, Windows Forms controls, GDI+ pens, brushes, and bitmaps.

There are, however, three scenarios for not disposing

- When you don’t “own” the object; for example, when obtaining a shared object via a static field or property
- When an object’s Dispose method does something that you don’t want
- When an object’s Dispose method is unnecessary by design, and disposing that object would add complexity to your program

<aside>
⚠️ Ignoring disposal can sometimes incur a performance cost

</aside>

### Clearing Fields in Disposal

In general, you don’t need to clear an object’s fields in its Dispose method. However, it is good practice to unsubscribe from events that the object has subscribed to internally over its lifetime

Unsubscribing from such events avoids receiving unwanted event notifications—and avoids unintentionally keeping the object alive in the eyes of the garbage collector (GC).

<aside>
📢 A Dispose method itself does not cause (managed) memory to be released—this can happen only in garbage collection.

</aside>

It’s also worth setting a field to indicate that the object is disposed so that you can throw an ObjectDisposedException if a consumer later tries to call members on the object. A good pattern is to use a publicly readable automatic property for this:

```csharp
public bool IsDisposed { get; private set; }
```

Although technically unnecessary, it can also be good to clear an object’s own event handlers (by setting them to null) in the Dispose method. This eliminates the possibility of those events firing during or after disposal.

### Anonymous Disposal

Sometimes, it’s useful to implement `IDisposable` without having to write a class. For instance, suppose that you want to expose methods on a class that suspend and resume event processing

```csharp
public class Disposable : IDisposable
{
 public static Disposable Create (Action onDispose)
 => new Disposable (onDispose);
 Action _onDispose;
 Disposable (Action onDispose) => _onDispose = onDispose;
 public void Dispose()
 {
 _onDispose?.Invoke(); // Execute disposal action if non-null.
 _onDispose = null; // Ensure it can't execute a second time.
 }
}
```

```csharp
public IDisposable SuspendEvents()
{
 _suspendCount++;
 return Disposable.Create (() => _suspendCount--);
}
```

# Automatic Garbage Collection

Regardless of whether an object requires a Dispose method for custom tear-down logic, at some point the memory it occupies on the heap must be freed. The CLR handles this side of it entirely automatically via an automatic GC. You never deallo‐ cate managed memory yourself. For example, consider the following method:

```csharp
public void Test()
{
 byte[] myArray = new byte[1000];
 ...
}
```

When Test executes, an array to hold 1,000 bytes is allocated on the memory heap. The array is referenced by the variable myArray, stored on the local variable stack. When the method exits, this local variable myArray pops out of scope, meaning that nothing is left to reference the array on the memory heap. The orphaned array then becomes eligible to be reclaimed in garbage collection.

<aside>
📢 In debug mode with optimizations disabled, the lifetime of an object referenced by a local variable extends to the end of the code block to ease debugging. Otherwise, it becomes eligible for collection at the earliest point at which it’s no longer used.

</aside>

Garbage collection does not happen immediately after an object is orphaned. Rather like garbage collection on the street, it happens periodically, although (unlike garbage collection on the street) not to a fixed schedule. The CLR bases its decision on when to collect upon a number of factors, such as the available memory, the amount of memory allocation, and the time since the last collection (the GC selftunes to optimize for an application’s specific memory access patterns). This means that **there’s an indeterminate delay between an object being orphaned and being released from memory. This delay can range from nanoseconds to days**.

### Garbage Collection and Memory Consumption

The GC tries to strike a balance between the time it spends doing garbage collection and the application’s memory consumption (working set). Consequently, applications can consume more memory than they need, particularly if large temporary arrays are constructed. You can monitor a process’s memory consumption via the Windows Task Manager or Resource Monitor or programmatically by querying a performance counter:

```csharp
// These types are in System.Diagnostics:
string procName = Process.GetCurrentProcess().ProcessName;
using PerformanceCounter pc = new PerformanceCounter
 ("Process", "Private Bytes", procName);
Console.WriteLine (pc.NextValue());
```

This queries the private working set, which gives the best overall indication of your program’s memory consumption. Specifically, it excludes memory that the CLR has internally deallocated and is willing to rescind to the OS should another process need it.

### Roots

A root is something that keeps an object alive. If an object is not directly or indirectly referenced by a root, it will be eligible for garbage collection.

A root is one of the following

- A local variable or parameter in an executing method (or in any method in its call stack)
- A static variable
- An object on the queue that stores objects ready for finalization (see the next section)

It’s impossible for code to execute in a deleted object, so if there’s any possibility of an (instance) method executing, its object must somehow be referenced in one of these ways.

Note that a group of objects that reference each other cyclically are considered dead without a root referee (see Figure 12-1). To put it in another way, objects that can‐ not be accessed by following the arrows (references) from a root object are unreach‐ able—and therefore subject to collection.

# Finalizers

Prior to an object being released from memory, its finalizer runs, if it has one. A finalizer is declared like a constructor, but it is prefixed by the `~` symbol:

```csharp
class Test
{
 ~Test()
 {
 // Finalizer logic...
 }
}
```

Finalizers are possible because garbage collection works in distinct phases. First, the GC identifies the unused objects ripe for deletion. Those without finalizers are deleted immediately. Those with pending (unrun) finalizers are kept alive (for now) and are put onto a special queue.

At that point, garbage collection is complete, and your program continues execut‐ ing. The finalizer thread then kicks in and starts running in parallel to your pro‐ gram, picking objects off that special queue and running their finalization methods. Prior to each object’s finalizer running, it’s still very much alive—that queue acts as a root object. After it’s been dequeued and the finalizer executed, the object becomes orphaned and will be deleted in the next collection (for that object’s generation).

Finalizers can be useful, but they come with some provisos:

- Finalizers slow the allocation and collection of memory (the GC needs to keep track of which finalizers have run).
- Finalizers prolong the life of the object and any referred objects (they must all await the next garbage truck for actual deletion).
- It’s impossible to predict in what order the finalizers for a set of objects will be called.
- You have limited control over when the finalizer for an object will be called.
- If code in a finalizer blocks, other objects cannot be finalized.
- Finalizers can be circumvented altogether if an application fails to unload cleanly.

Here are some guidelines for implementing finalizers:

- Ensure that your finalizer executes quickly
- Never block in your finalizer
- Don’t reference other finalizable objects.
- Don’t throw exceptions.

### Calling Dispose from a Finalizer

A popular pattern is to have the finalizer call Dispose. This makes sense when cleanup is not urgent and hastening it by calling Dispose is more of an optimization than a necessity

<aside>
⚠️ Keep in mind that with this pattern you couple memory deal‐ location to resource deallocation—two things with potentially divergent interests (unless the resource is itself memory). You also increase the burden on the finalization thread.

</aside>

```csharp
class Test : IDisposable
{
 public void Dispose() // NOT virtual
 {
 Dispose (true);
 GC.SuppressFinalize (this); // Prevent finalizer from running.
 }
 protected virtual void Dispose (bool disposing)
 {
 if (disposing)
 {
 // Call Dispose() on other objects owned by this instance.
 // You can reference other finalizable objects here.
 // ...
 }
 // Release unmanaged resources owned by (just) this object.
 // ...
 }
 ~Test() => Dispose (false);
}
```

To make this robust, any code capable of throwing an exception should be wrapped in a try/catch block, and the exception, ideally, logged. Any logging should be as simple and robust as possible.

### Resurrection

Suppose a finalizer modifies a living object such that it refers back to the dying object. When the next garbage collection happens (for the object’s generation), the CLR will see the previously dying object as no longer orphaned—and so it will evade garbage collection. This is an advanced scenario, and is called resurrection.

To illustrate, suppose that we want to write a class that manages a temporary file. When an instance of that class is garbage-collected, we’d like the finalizer to delete the temporary file. It sounds easy:

```csharp
public class TempFileRef
{
 public readonly string FilePath;
 public TempFileRef (string filePath) { FilePath = filePath; }
 ~TempFileRef() { File.Delete (FilePath); }
}
```

Unfortunately, this has a bug: File.Delete might throw an exception (due to a lack of permissions, perhaps, or the file being in use, or having already been deleted). Such an exception would take down the entire application (as well as preventing other finalizers from running). We could simply “swallow” the exception with an empty catch block, but then we’d never know that anything went wrong. Calling some elaborate error-reporting API would also be undesirable because it would bur‐ den the finalizer thread, hindering garbage collection for other objects. We want to restrict finalization actions to those that are simple, reliable, and quick.

```csharp
public class TempFileRef
{
 static internal readonly ConcurrentQueue<TempFileRef> FailedDeletions
 = new ConcurrentQueue<TempFileRef>();
 public readonly string FilePath;
 public Exception DeletionError { get; private set; }
 public TempFileRef (string filePath) { FilePath = filePath; }
 ~TempFileRef()
 {
try { File.Delete (FilePath); }
 catch (Exception ex)
 {
 DeletionError = ex;
 FailedDeletions.Enqueue (this); // Resurrection
 }
 }
}
```

**Enqueuing the object to the static FailedDeletions collection gives the object another referee, ensuring that it remains alive until the object is eventually dequeued.**

### How the GC Works

The standard CLR uses a generational mark-and-compact GC that performs automatic memory management for objects stored on the managed heap. The GC is considered to be a tracing GC in that it doesn’t interfere with every access to an object, but rather wakes up intermittently and traces the graph of objects stored on the managed heap to determine which objects can be considered garbage and there‐ fore collected.

The GC initiates a garbage collection upon performing a memory allocation (via the new keyword), either after a certain threshold of memory has been allocated or at other times to reduce the application’s memory footprint. This process can also be initiated manually by calling `System.GC.Collect`. During a garbage collection, all threads can be frozen (more on this in the next section).

The GC begins with its root object references and walks the object graph, marking all the objects it touches as reachable. When this process is complete, all objects that have not been marked are considered unused and are subject to garbage collection.

Unused objects without finalizers are immediately discarded; unused objects with finalizers are enqueued for processing on the finalizer thread after the GC is complete. These objects then become eligible for collection in the next GC for the object’s generation (unless resurrected).

The remaining “live” objects are then shifted to the start of the heap (compacted), freeing space for more objects. This compaction serves two purposes: it avoids memory fragmentation, and it allows the GC to employ a very simple strategy when allocating new objects, which is to always allocate memory at the end of the heap. This avoids the potentially time-consuming task of maintaining a list of free memory segments.

If there is insufficient space to allocate memory for a new object after garbage col‐ lection and the OS is unable to grant further memory, an `OutOfMemoryException` is thrown.

### Managed Memory Leaks

In unmanaged languages such as C++, you must remember to manually deallocate memory when an object is no longer required; otherwise, a memory leak will result. In the managed world, this kind of error is impossible due to the CLR’s automatic garbage collection system.

Nonetheless, large and complex .NET applications can exhibit a milder form of the same syndrome with the same end result: the application consumes more and more memory over its lifetime, until it eventually must be restarted. The good news is that managed memory leaks are usually easier to diagnose and prevent.

Managed memory leaks are caused by unused objects remaining alive by virtue of unused or forgotten references. A common candidate is event handlers—these hold a reference to the target object (unless the target is a static method).

```csharp
class Host
{
 public event EventHandler Click;
}
class Client
{
 Host _host;
 public Client (Host host)
{
 _host = host;
 _host.Click += HostClicked;
 }
 void HostClicked (object sender, EventArgs e) { ... }
}
```

The following test class contains a method that instantiates 1,000 clients

```csharp
class Test
{
 static Host _host = new Host();
 public static void CreateClients()
 {
 Client[] clients = Enumerable.Range (0, 1000)
 .Select (i => new Client (_host))
 .ToArray();
 // Do something with clients ...
 }
}
```

You might expect that after CreateClients finishes executing, the 1,000 Client objects will become eligible for collection. Unfortunately, each client has another referee: the _host object whose Click event now references each Client instance. This can go unnoticed if the Click event doesn’t fire—or if the HostClicked method doesn’t do anything to attract attention.

One way to solve this is to make Client implement IDisposable, and in the Dispose method, unhook the event handler:

```csharp
public void Dispose() { _host.Click -= HostClicked; }
```

Consumers of Client then dispose of the instances when they’re done with them:

```csharp
Array.ForEach (clients, c => c.Dispose());
```

### Diagnosing Memory Leaks

The easiest way to avoid managed memory leaks is to proactively monitor memory consumption as an application is written. You can obtain the current memory consumption of a program’s objects as follows (the true argument tells the GC to perform a collection first):

```csharp
long memoryUsed = GC.GetTotalMemory (true);
```

**If you’re practicing test-driven development, one possibility is to use unit tests to assert that memory is reclaimed as expected. If such an assertion fails, you then need examine only the changes that you’ve made recently**# Chapter 13 : Diagnostics

# Overview

When things go wrong, it’s important that information is available to aid in diagnos‐ ing the problem. An Integrated Development Environment (IDE) or debugger can assist greatly to this effect—but it is usually available only during development. After an application ships, the application itself must gather and record diagnostic information. To meet this requirement, .NET Core provides a set of facilities to log diagnostic information, monitor application behavior, detect runtime errors, and integrate with debugging tools if available.

Some diagnostic tools and APIs are Windows specific because they rely on features of the Windows operating system. In an effort to prevent platform-specific APIs from cluttering .NET Core, Microsoft has shipped them in separate NuGet packages that you can optionally reference.

# Conditional Compilation

You can conditionally compile any section of code in C# with preprocessor directives. Preprocessor directives are special instructions to the compiler that begin with the # symbol (and, unlike other C# constructs, must appear on a line of their own). Logically, they execute before the main compilation takes place (although in practice, the compiler processes them during the lexical parsing phase). The preprocessor directives for conditional compilation are `#if`, `#else`, `#endif`, and `#elif`.

The #if directive instructs the compiler to ignore a section of code unless a specified symbol has been defined. You can define a symbol in source code by using the `#define` directive (in which case the symbol applies to just that file), or in the .csproj file by using a `<DefineConstants>` element (in which case the symbol applies to whole assembly):

```csharp
#define TESTMODE // #define directives must be at top of file
 // Symbol names are uppercase by convention.
using System;
class Program
{
 static void Main()
 {
#if TESTMODE
 Console.WriteLine ("in test mode!"); // OUTPUT: in test mode!
#endif
 }
}
```

If we deleted the first line, the program would compile with the Console.WriteLine statement completely eliminated from the executable, as though it were commented out.

The `#else` statement is analogous to C#’s else statement, and `#elif` is equivalent to `#else` followed by `#if`. The `||`, `&&`, and `!` operators perform or, and, and not operations:

```csharp
#if TESTMODE && !PLAYMODE // if TESTMODE and not PLAYMODE
 ...
```

You can define symbols that apply to every file in an assembly by editing the .csproj file (or in Visual Studio, by going to the Build tab in the Project Properties window). The following defines two constants, TESTMODE and PLAYMODE:

```csharp
<PropertyGroup>
 <DefineConstants>TESTMODE;PLAYMODE</DefineConstants>
</PropertyGroup>
```

If you’ve defined a symbol at the assembly level and then want to “undefine” it for a particular file, you can do so by using the `#undef` directive.

### Conditional Compilation Versus Static Variable Flags

You could instead implement the preceding example with a simple static field

```csharp
static internal bool TestMode = true;
static void Main()
{
 if (TestMode) Console.WriteLine ("in test mode!");
}
```

This has the advantage of allowing runtime configuration. So, why choose condi‐ tional compilation? The reason is that conditional compilation can take you places variable flags cannot, such as the following:

- Conditionally including an attribute
- Changing the declared type of variable
- Switching between different namespaces or type aliases in a using directive

```csharp
using TestType =
 #if V2
 MyCompany.Widgets.GadgetV2;
 #else
 MyCompany.Widgets.Gadget;
 #endif
```

You can even perform major refactoring under a conditional compilation directive,
so you can instantly switch between old and new versions, and write libraries that
can compile against multiple Framework versions, leveraging the latest Framework
features where available.

Another advantage of conditional compilation is that debugging code can refer to types in assemblies that are not included in deployment.

### The Conditional Attribute

The Conditional attribute instructs the compiler to ignore any calls to a particular class or method, if the specified symbol has not been defined.

To see how this is useful, suppose that you write a method for logging status infor‐ mation as follows:

```csharp
static void LogStatus (string msg)
{
 string logFilePath = ...
 System.IO.File.AppendAllText (logFilePath, msg + "\r\n");
}
```

Now imagine that you want this to execute only if the LOGGINGMODE symbol is defined. The first solution is to wrap all calls to LogStatus around an #if directive:

```csharp
#if LOGGINGMODE
LogStatus ("Message Headers: " + GetMsgHeaders());
#endif
```

This gives an ideal result, but it is tedious. The second solution is to put the #if directive inside the LogStatus method. This, however, is problematic should Log Status be called as follows:

```csharp
LogStatus ("Message Headers: " + GetComplexMessageHeaders());
```

GetComplexMessageHeaders would always be called—which might incur a performance hit.

We can combine the functionality of the first solution with the convenience of the second by attaching the Conditional attribute (defined in System.Diagnostics) to the LogStatus method:

```csharp
[Conditional ("LOGGINGMODE")]
static void LogStatus (string msg)
{
 ...
}
```

```csharp
#define TESTMODE            // #define directives must be at top of file
#define PLAYMODE
//#undef PLAYMODE  // Cancels our define above if not commented out. Also cancels a define from the compiler e.g. through Visual Studio settings.
#define LOGGINGMODE

// Symbol names are uppercase by convention.

class Program
{
    static void Main()
    {
#if TESTMODE
    Console.WriteLine ("in test mode!"); 
#endif
#if TESTMODE && !PLAYMODE      // if TESTMODE and not PLAYMODE
    Console.WriteLine ("Test mode and NOT play mode"); 
#endif
#if PLAYMODE
    Console.WriteLine ("Play mode is defined.");
#endif
    LogStatus("Only if LOGGINGMODE is defined.");
    }

    [Conditional ("LOGGINGMODE")]
    static void LogStatus (string msg)
    {
        Console.WriteLine($"LOG: {msg}");
    }

}
```

### Alternatives to the Conditional attribute

The Conditional attribute is useless if you need to dynamically enable or disable functionality at runtime: instead, you must use a variable-based approach. This leaves the question of how to elegantly circumvent the evaluation of arguments when calling conditional logging methods. A functional approach solves this:

```csharp
using System;
using System.Linq;
class Program
{
 public static bool EnableLogging;
 static void LogStatus (Func<string> message)
 {
 string logFilePath = ...
 if (EnableLogging)
 System.IO.File.AppendAllText (logFilePath, message() + "\r\n");
 }
}
//A lambda expression lets you call this method without syntax bloat:
LogStatus ( () => "Message Headers: " + GetComplexMessageHeaders() );
//If EnableLogging is false, GetComplexMessageHeaders is never evaluated.
```

# Debug and Trace Classes

Debug and Trace are static classes that provide basic logging and assertion capabilities. The two classes are very similar; the main differentiator is their intended use. The Debug class is intended for debug builds; the Trace class is intended for both debug and release builds. To this effect:

- All methods of the Debug class are defined with [Conditional("DEBUG")].
- All methods of the Trace class are defined with [Conditional("TRACE")].

This means that all calls that you make to Debug or Trace are eliminated by the compiler unless you define DEBUG or TRACE symbols. (Visual Studio provides check‐ boxes for defining these symbols in the Build tab of Project Properties and enables the TRACE symbol by default with new projects.)

Both the Debug and Trace classes provide Write, WriteLine, and WriteIf methods. By default, these send messages to the debugger’s output window:

```csharp
Debug.Write ("Data");
Debug.WriteLine (23 * 34);
int x = 5, y = 3;
Debug.WriteIf (x > y, "x is greater than y");
```

### Fail and Assert

The Debug and Trace classes both provide Fail and Assert methods. Fail sends the message to each TraceListener in the Debug or Trace class’s Listeners collec‐ tion (see the following section), which by default writes the message to the debug output:

```csharp
Debug.Fail ("File data.txt does not exist!");
```

Assert simply calls Fail if the bool argument is false—this is called making an assertion and indicates a bug in the code if violated. Specifying a failure message is optional:

```csharp
Debug.Assert (File.Exists ("data.txt"), "File data.txt does not exist!");
var result = ...
Debug.Assert (result != null);
```

### TraceListener

The Trace class has a static Listeners property that returns a collection of Trace Listener instances. These are responsible for processing the content emitted by the Write, Fail, and Trace methods.

By default, the Listeners collection of each includes a single listener (Default TraceListener). The default listener has two key features:

- When connected to a debugger such as Visual Studio, messages are written to the debug output window; otherwise, message content is ignored.
- When the Fail method is called (or an assertion fails), the application is terminated.

```csharp
// Clear the default listener:
Trace.Listeners.Clear();
// Add a writer that appends to the trace.txt file:
Trace.Listeners.Add (new TextWriterTraceListener ("trace.txt"));
// Obtain the Console's output stream, then add that as a listener:
System.IO.TextWriter tw = Console.Out;
Trace.Listeners.Add (new TextWriterTraceListener (tw));
// Set up a Windows Event log source and then create/add listener.
// CreateEventSource requires administrative elevation, so this would
// typically be done in application setup.
if (!EventLog.SourceExists ("DemoApp"))
 EventLog.CreateEventSource ("DemoApp", "Application");
Trace.Listeners.Add (new EventLogTraceListener ("DemoApp"));
```

# Debugger Integration

Sometimes, it’s useful for an application to interact with a debugger if one is avail‐ able. During development, the debugger is usually your IDE (e.g., Visual Studio); in deployment, the debugger is more likely to be one of the lower-level debugging tools, such as WinDbg, Cordbg, or Mdbg.

### Attaching and Breaking

The static Debugger class in System.Diagnostics provides basic functions for inter‐ acting with a debugger—namely Break, Launch, Log, and IsAttached.

A debugger must first attach to an application in order to debug it. If you start an application from within an IDE, this happens automatically, unless you request otherwise (by choosing “Start without debugging”). Sometimes, though, it’s inconvenient or impossible to start an application in debug mode within the IDE. An example is a Windows Service application or (ironically) a Visual Studio designer. One solution is to start the application normally and then, in your IDE, choose Debug Process. This doesn’t allow you to set breakpoints early in the program’s execution, however.

### Debugger Attributes

The DebuggerStepThrough and DebuggerHidden attributes provide suggestions to the debugger on how to handle single-stepping for a particular method, constructor, or class.

DebuggerStepThrough requests that the debugger step through a function without any user interaction. This attribute is useful in automatically generated methods and in proxy methods that forward the real work to a method somewhere else. In the latter case, the debugger will still show the proxy method in the call stack if a break‐ point is set within the “real” method—unless you also add the DebuggerHidden attribute.

```csharp
[DebuggerStepThrough, DebuggerHidden]
void DoWorkProxy()
{
 // setup...
 DoWork();
 // teardown...
}
void DoWork() {...} // Real method...
```

### Processes and Process Threads

### Examining Running Processes

The Process.GetProcessXXX methods retrieve a specific process by name or pro‐ cess ID, or all processes running on the current or nominated computer. This includes both managed and unmanaged processes. Each Process instance has a wealth of properties mapping statistics such as name, ID, priority, memory and pro‐ cessor utilization, window handles, and so on. The following sample enumerates all the running processes on the current computer:

```csharp
foreach (Process p in Process.GetProcesses())
using (p)
{
 Console.WriteLine (p.ProcessName);
 Console.WriteLine (" PID: " + p.Id);
 Console.WriteLine (" Memory: " + p.WorkingSet64);
 Console.WriteLine (" Threads: " + p.Threads.Count);
}
```

### Examining Threads in a Process

You can also enumerate over the threads of other processes, with the Process.Threads property. The objects that you get, however, are not System .Threading.Thread objects; they’re ProcessThread objects and are intended for administrative rather than synchronization tasks. A ProcessThread object provides diagnostic information about the underlying thread and allows you to control some aspects of it such as its priority and processor affinity:

```csharp
public void EnumerateThreads (Process p)
{
 foreach (ProcessThread pt in p.Threads)
 {
 Console.WriteLine (pt.Id);
 Console.WriteLine (" State: " + pt.ThreadState);
 Console.WriteLine (" Priority: " + pt.PriorityLevel);
 Console.WriteLine (" Started: " + pt.StartTime);
 Console.WriteLine (" CPU time: " + pt.TotalProcessorTime);
 }
}
```

# StackTrace and StackFrame

The StackTrace and StackFrame classes provide a read-only view of an execution call stack. You can obtain stack traces for the current thread or an Exception object. Such information is useful mostly for diagnostic purposes, though you also can use it in programming (hacks). StackTrace represents a complete call stack; Stack Frame represents a single method call within that stack.

If you instantiate a StackTrace object with no arguments—or with a bool argument—you get a snapshot of the current thread’s call stack. The bool argument, if true, instructs StackTrace to read the assembly .pdb (project debug) files if they are present, giving you access to filename, line number, and column offset data. Project debug files are generated when you compile with the /debug switch. (Visual Studio compiles with this switch unless you request otherwise via Advanced Build Settings.)

After you’ve obtained a StackTrace, you can examine a particular frame by calling GetFrame—or obtain the whole lot by using GetFrames:

```csharp
static void Main() { A (); }
static void A() { B (); }
static void B() { C (); }
static void C()
{
 StackTrace s = new StackTrace (true);
Console.WriteLine ("Total frames: " + s.FrameCount);
 Console.WriteLine ("Current method: " + s.GetFrame(0).GetMethod().Name);
 Console.WriteLine ("Calling method: " + s.GetFrame(1).GetMethod().Name);
 Console.WriteLine ("Entry method: " + s.GetFrame
 (s.FrameCount-1).GetMethod().Name);
 Console.WriteLine ("Call Stack:");
 foreach (StackFrame f in s.GetFrames())
 Console.WriteLine (
 " File: " + f.GetFileName() +
 " Line: " + f.GetFileLineNumber() +
 " Col: " + f.GetFileColumnNumber() +
 " Offset: " + f.GetILOffset() +
 " Method: " + f.GetMethod().Name);
}
```

```csharp
Here’s the output:
Total frames: 4
Current method: C
Calling method: B
Entry method: Main
Call stack:
 File: C:\Test\Program.cs Line: 15 Col: 4 Offset: 7 Method: C
 File: C:\Test\Program.cs Line: 12 Col: 22 Offset: 6 Method: B
 File: C:\Test\Program.cs Line: 11 Col: 22 Offset: 6 Method: A
 File: C:\Test\Program.cs Line: 10 Col: 25 Offset: 6 Method: Main
```

# Windows Event Logs

The Win32 platform provides a centralized logging mechanism, in the form of the Windows event logs.

The Debug and Trace classes we used earlier write to a Windows event log if you register an EventLogTraceListener. With the EventLog class, however, you can write directly to a Windows event log without using Trace or Debug. You can also use this class to read and monitor event data.

<aside>
⚠️ Writing to the Windows event log makes sense in a Windows Service application, because if something goes wrong, you can’t pop up a user interface directing the user to some special file where diagnostic information has been written. Also, because it’s common practice for services to write to the Windows event log, this is the first place an administrator is likely to look if your service falls over.

</aside>

There are three standard Windows event logs, identified by these names:

- Application
- System
- Security

### Writing to the Event Log

- Choose one of the three event logs (usually Application).
- Decide on a source name and create it if necessary (create requires administrative permissions).
- Call EventLog.WriteEntry with the log name, source name, and message data

The source name is an easily identifiable name for your application. You must register a source name before you use it the CreateEventSource method performs this function. You can then call WriteEntry:

```csharp
const string SourceName = "MyCompany.WidgetServer";
// CreateEventSource requires administrative permissions, so this would
// typically be done in application setup.
if (!EventLog.SourceExists (SourceName))
 EventLog.CreateEventSource (SourceName, "Application");
EventLog.WriteEntry (SourceName,
 "Service started; using configuration file=...",
 EventLogEntryType.Information);
```

EventLogEntryType can be Information, Warning, Error, SuccessAudit, or FailureAudit. Each displays with a different icon in the Windows event viewer. You can also optionally specify a category and event ID (each is a number of your own choosing) and provide optional binary data.

### Reading the Event Log

To read an event log, instantiate the EventLog class with the name of the log that you want to access and optionally the name of another computer on which the log resides. Each log entry can then be read via the Entries collection property:

```csharp
EventLog log = new EventLog ("Application");
Console.WriteLine ("Total entries: " + log.Entries.Count);
EventLogEntry last = log.Entries [log.Entries.Count - 1];
Console.WriteLine ("Index: " + last.Index);
Console.WriteLine ("Source: " + last.Source);
Console.WriteLine ("Type: " + last.EntryType);
Console.WriteLine ("Time: " + last.TimeWritten);
Console.WriteLine ("Message: " + last.Message);
```

You can enumerate over all logs for the current (or another) computer via the static method `EventLog.GetEventLogs()`

```csharp
foreach (EventLog log in EventLog.GetEventLogs())
 Console.WriteLine (log.LogDisplayName);
```

### Monitoring the Event Log

You can be alerted whenever an entry is written to a Windows event log, via the EntryWritten event. This works for event logs on the local computer, and it fires regardless of what application logged the event.

```csharp
static void Main()
{
 using (var log = new EventLog ("Application"))
 {
 log.EnableRaisingEvents = true;
 log.EntryWritten += DisplayEntry;
 Console.ReadLine();
 }
}
static void DisplayEntry (object sender, EntryWrittenEventArgs e)
{
 EventLogEntry entry = e.Entry;
 Console.WriteLine (entry.Message);
}
```

# Performance Counters

The logging mechanisms we’ve discussed to date are useful for capturing informa‐ tion for future analysis. However, to gain insight into the current state of an applica‐ tion (or the system as a whole), a more real-time approach is needed. The Win32 solution to this need is the performance-monitoring infrastructure, which consists of a set of performance counters that the system and applications expose, and the Microsoft Management Console (MMC) snap-ins used to monitor these counters in real time.

Performance counters are grouped into categories such as System, Processor, .NET CLR Memory, and so on. These categories are sometimes also referred to as perfor‐ mance objects by the GUI tools. Each category groups a related set of performance counters that monitor one aspect of the system or application. Examples of perfor‐ mance counters in the .NET CLR Memory category include “% Time in GC,” “# Bytes in All Heaps,” and “Allocated bytes/sec.”

Each category can optionally have one or more instances that can be monitored independently. For example, this is useful in the “% Processor Time” performance counter in the Processor category, which allows one to monitor CPU utilization. On a multiprocessor machine, this counter supports an instance for each CPU, allowing you to monitor the utilization of each CPU independently

### Enumerating the Available Counters

The following example enumerates over all of the available performance counters on the computer. For those that have instances, it enumerates the counters for each instance:

```csharp
PerformanceCounterCategory[] cats =
 PerformanceCounterCategory.GetCategories();
foreach (PerformanceCounterCategory cat in cats)
{
 Console.WriteLine ("Category: " + cat.CategoryName);
 string[] instances = cat.GetInstanceNames();
 if (instances.Length == 0)
 {
 foreach (PerformanceCounter ctr in cat.GetCounters())
 Console.WriteLine (" Counter: " + ctr.CounterName);
 }
 else // Dump counters with instances
 {
 foreach (string instance in instances)
 {
 Console.WriteLine (" Instance: " + instance);
 if (cat.InstanceExists (instance))
 foreach (PerformanceCounter ctr in cat.GetCounters (instance))
 Console.WriteLine (" Counter: " + ctr.CounterName);
 }
 }
}
```

### Reading Performance Counter Data

To retrieve the value of a performance counter, instantiate a PerformanceCounter object and then call the NextValue or NextSample method. NextValue returns a simple float value; NextSample returns a CounterSample object that exposes a more advanced set of properties, such as CounterFrequency, TimeStamp, BaseValue, and RawValue.

PerformanceCounter’s constructor takes a category name, counter name, and optional instance. So, to display the current processor utilization for all CPUs, you would do the following:

```csharp
using PerformanceCounter pc = new PerformanceCounter ("Processor",
 "% Processor Time",
 "_Total");
Console.WriteLine (pc.NextValue());
```

```csharp
string procName = Process.GetCurrentProcess().ProcessName;
using PerformanceCounter pc = new PerformanceCounter ("Process",
 "Private Bytes",
 procName);
Console.WriteLine (pc.NextValue());
```

PerformanceCounter doesn’t expose a ValueChanged event, so if you want to moni‐ tor for changes, you must poll. In the next example, we poll every 200 milliseconds—until signaled to quit by an EventWaitHandle:

```csharp
// need to import System.Threading as well as System.Diagnostics
static void Monitor (string category, string counter, string instance,
 EventWaitHandle stopper)
{
 if (!PerformanceCounterCategory.Exists (category))
 throw new InvalidOperationException ("Category does not exist");
 if (!PerformanceCounterCategory.CounterExists (counter, category))
 throw new InvalidOperationException ("Counter does not exist");
 if (instance == null) instance = ""; // "" == no instance (not null!)
 if (instance != "" &&
 !PerformanceCounterCategory.InstanceExists (instance, category))
 throw new InvalidOperationException ("Instance does not exist");
 float lastValue = 0f;
 using (PerformanceCounter pc = new PerformanceCounter (category,
 counter, instance))
 while (!stopper.WaitOne (200, false))
 {
 float value = pc.NextValue();
 if (value != lastValue) // Only write out the value
 { // if it has changed.
 Console.WriteLine (value);
 lastValue = value;
 }
 }
}
```

Here’s how we can use this method to simultaneously monitor processor and harddrive activity:

```csharp
static void Main()
{
 EventWaitHandle stopper = new ManualResetEvent (false);
 new Thread (() =>
 Monitor ("Processor", "% Processor Time", "_Total", stopper)
 ).Start();
 new Thread (() =>
 Monitor ("LogicalDisk", "% Idle Time", "C:", stopper)
 ).Start();
 Console.WriteLine ("Monitoring - press any key to quit");
 Console.ReadKey();
 stopper.Set();
}
```

# The Stopwatch Class

The Stopwatch class provides a convenient mechanism for measuring execution times. Stopwatch uses the highest-resolution mechanism that the OS and hardware provide, which is typically less than a microsecond. (In contrast, DateTime.Now and Environment.TickCount have a resolution of about 15 ms.)

To use Stopwatch, call StartNew—this instantiates a Stopwatch and starts it ticking. (Alternatively, you can instantiate it manually and then call Start.) The Elapsed property returns the elapsed interval as a TimeSpan:

```csharp
Stopwatch s = Stopwatch.StartNew();
System.IO.File.WriteAllText ("test.txt", new string ('*', 30000000));
Console.WriteLine (s.Elapsed); // 00:00:01.4322661
```# Chapter 14 : Concurrency and Asynchrony

# Introduction

Following are the most common concurrency scenarios

- Writing a responsive user interface
    
    In WPF, mobile, and Windows Forms applications, you must run timeconsuming tasks concurrently with the code that runs your user interface to maintain responsiveness.
    
- Allowing requests to process simultaneously
    
    On a server, client requests can arrive concurrently and so must be handled in parallel to maintain scalability. If you use [ASP.NET](http://asp.net/) Core or Web API, .NET Core does this for you automatically. However, you still need to be aware of shared state (for instance, the effect of using static variables for caching).
    
- Parallel programming
    
    Code that performs intensive calculations can execute faster on multicore/ multiprocessor computers if the workload is divided between cores
    
- Speculative execution
    
    On multicore machines, you can sometimes improve performance by predicting something that might need to be done and then doing it ahead of time. LINQPad uses this technique to speed up the creation of new queries. A variation is to run a number of different algorithms in parallel that all solve the same task. Whichever one finishes first “wins” this is effective when you can’t know ahead of time which algorithm will execute fastest.
    

# Threading

A thread is an execution path that can **proceed independently of others**.

Each thread runs within an operating system process, which provides an isolated environment in which a program runs. With a single-threaded program, just one thread runs in the process’s isolated environment and so that thread has exclusive access to it. **With a multithreaded program, multiple threads run in a single process, sharing the same execution environment** (memory, in particular). This, in part, is why multithreading is useful: one thread can fetch data in the background, for instance, while another thread displays the data as it arrives. This data is referred to as shared state.

## Creating a Thread

A client program (Console, WPF, UWP, or Windows Forms) starts in a single thread that’s created automatically by the OS (the “main” thread). Here it lives out its life as a single-threaded application, unless you do otherwise, by creating more threads

You can create and start a new thread by instantiating a Thread object and calling its Start method. The simplest constructor for Thread takes a ThreadStart delegate: a parameterless method indicating where execution should begin.

```csharp
// NB: All samples in this chapter assume the following namespace imports:
using System;
using System.Threading;
class ThreadTest
{
 static void Main()
 {
 Thread t = new Thread (WriteY); // Kick off a new thread
 t.Start(); // running WriteY()
 // Simultaneously, do something on the main thread.
 for (int i = 0; i < 1000; i++) Console.Write ("x");Thread.Sleep(100);}
 static void WriteY()
 {
 for (int i = 0; i < 1000; i++) Console.Write ("y");Thread.Sleep(100);
 }
}
// Typical Output:
xxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyy
yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
yyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
...
```

The main thread creates a new thread t on which it runs a method that repeatedly prints the character y. Simultaneously, the main thread repeatedly prints the character x, as shown in Figure 14-1. On a single-core computer, the operating system must allocate “slices” of time to each thread (typically 20 ms in Windows) to simulate concurrency, resulting in repeated blocks of x and y. On a multicore or multiprocessor machine, the two threads can genuinely execute in parallel (subject to competition by other active processes on the computer), although you still get repeated blocks of x and y in this example because of subtleties in the mechanism by which Console handles concurrent requests.


After it’s started, a thread’s IsAlive property returns true, until the point at which the thread ends. A thread ends when the delegate passed to the Thread’s constructor finishes executing. After it’s ended, a thread cannot restart.

Each thread has a Name property that you can set for the benefit of debugging. This is particularly useful in Visual Studio because the thread’s name is displayed in the Threads Window and Debug Location toolbar. You can set a thread’s name just once; attempts to change it later will throw an exception. The static `Thread.CurrentThread` property gives you the currently executing thread:

```csharp
Console.WriteLine (Thread.CurrentThread.Name);
```

## Join and Sleep

You can wait for another thread to end by calling its Join method

```csharp
static void Main()
{
 Thread t = new Thread (Go);
 t.Start();
 t.Join();
 Console.WriteLine ("Thread t has ended!");
}
static void Go() { for (int i = 0; i < 1000; i++) Console.Write ("y"); }
```

This prints “y” 1,000 times, followed by “Thread t has ended!” immediately after‐ ward. You can include a timeout when calling Join, either in milliseconds or as a TimeSpan. It then returns true if the thread ended or false if it timed out.

Thread.Sleep pauses the current thread for a specified period:

```csharp
Thread.Sleep (TimeSpan.FromHours (1)); // Sleep for 1 hour
Thread.Sleep (500); // Sleep for 500 milliseconds
```

Thread.Sleep(0) relinquishes the thread’s current time slice immediately, voluntarily handing over the CPU to other threads. Thread.Yield() does the same thing except that it relinquishes only to threads running on the same processor

While waiting on a Sleep or Join, a thread is blocked.

## Blocking

A thread is deemed blocked when its execution is paused for some reason, such as when Sleeping or waiting for another to end via Join. A blocked thread immediately yields its processor time slice, and from then on it consumes no processor time until its blocking condition is satisfied. You can test for a thread being blocked via its ThreadState property:

```csharp
bool blocked = (someThread.ThreadState & ThreadState.WaitSleepJoin) != 0;
```

ThreadState is a flags enum, combining three “layers” of data in a bitwise fashion. Most values, however, are redundant, unused, or deprecated. The following extension method strips a ThreadState to one of four useful values: Unstarted, Running, WaitSleepJoin, and Stopped:

```csharp
public static ThreadState Simplify (this ThreadState ts)
{
 return ts & (ThreadState.Unstarted |
 ThreadState.WaitSleepJoin |
 ThreadState.Stopped);
}
```

The ThreadState property is useful for diagnostic purposes but unsuitable for synchronization, because a thread’s state can change in between testing ThreadState and acting on that information.

When a thread blocks or unblocks, the OS performs a context switch. This incurs a small overhead, typically one or two microseconds.

### I/O-bound versus compute-bound

An operation that spends most of its time waiting for something to happen is called I/O-bound—an example is downloading a web page or calling Console.ReadLine. (I/O-bound operations typically involve input or output, but this is not a hard requirement: Thread.Sleep is also deemed I/O-bound.) In contrast, an operation that spends most of its time performing CPU-intensive work is called compute-bound.

### Blocking versus spinning

An I/O-bound operation works in one of two ways: it either waits synchronously on the current thread until the operation is complete (such as Console.ReadLine, Thread.Sleep, or Thread.Join), or operates asynchronously, firing a callback when the operation finishes some time thereafter (more on this later).

I/O-bound operations that wait synchronously spend most of their time blocking a thread. They can also “spin” in a loop periodically:

```csharp
while (DateTime.Now < nextStartTime)
 Thread.Sleep (100);
```

Leaving aside that there are better ways to do this (such as timers or signaling con‐ structs), another option is that a thread can spin continuously:

```csharp
while (DateTime.Now < nextStartTime);
```

In general, this is very wasteful on processor time: as far as the CLR and OS are concerned, the thread is performing an important calculation and thus is allocated resources accordingly. In effect, we’ve turned what should be an I/O-bound operation into a compute-bound operation.

<aside>
👉🏿 There are a couple of nuances with regard to spinning versus blocking. First, spinning very briefly can be effective when you expect a condition to be satisfied soon (perhaps within a few microseconds) because it avoids the overhead and latency of a context switch.

</aside>

<aside>
👉🏿 Second, blocking does not incur a zero cost. This is because each thread ties up around 1 MB of memory for as long as it lives and causes an ongoing administrative overhead for the CLR and OS. For this reason, blocking can be troublesome in the context of heavily I/O-bound programs that need to handle hundreds or thousands of concurrent operations. Instead, such programs need to use a callback-based approach, rescinding their thread entirely while waiting.

</aside>

## Local versus Shared State

The CLR assigns each thread its own memory stack so that local variables are kept separate. In the next example, we define a method with a local variable and then call the method simultaneously on the main thread and a newly created thread:

```csharp
static void Main()
{
 new Thread (Go).Start(); // Call Go() on a new thread
 Go(); // Call Go() on the main thread
}
static void Go()
{
 // Declare and use a local variable - 'cycles'
 for (int cycles = 0; cycles < 5; cycles++) Console.Write ('?');
}
```

A separate copy of the cycles variable is created on each thread’s memory stack, and so the output is, predictably, 10 question marks

Threads share data if they have a common reference to the same object instance:

```csharp
class ThreadTest
{
 bool _done;
 static void Main()
 {
 ThreadTest tt = new ThreadTest(); // Create a common instance
 new Thread (tt.Go).Start();
 tt.Go();
 }
 void Go() // Note that this is an instance method
 {
 if (!_done) { _done = true; Console.WriteLine ("Done"); }
}
}
```

Because both threads call Go() on the same ThreadTest instance, they share the _done field. This results in “Done” being printed once instead of twice.

Local variables captured by a lambda expression or anonymous delegate are con‐ verted by the compiler into fields and so can also be shared

```csharp
static void Main()
{
 bool done = false;
 ThreadStart action = () =>
 {
 if (!done) { done = true; Console.WriteLine ("Done"); }
 };
 new Thread (action).Start();
 action();
}
```

Static fields offer another way to share data between threads:

```csharp
class ThreadTest
{
 static bool _done; // Static fields are shared between all threads
 // in the same application domain.
 static void Main()
 {
 new Thread (Go).Start();
 Go();
 }
 static void Go()
 {
 if (!_done) { _done = true; Console.WriteLine ("Done"); }
 }
}
```

All three examples illustrate another key concept: that of thread safety (or rather, lack of it!). The output is actually indeterminate: it’s possible (though unlikely) that “Done” could be printed twice. If, however, we swap the order of statements in the Go method, the odds of “Done” being printed twice go up dramatically

```csharp
static void Go()
{
 if (!_done) { Console.WriteLine ("Done"); _done = true; }
}
```

The problem is that one thread can be evaluating the if statement at exactly the same time as the other thread is executing the WriteLine statement—before it’s had a chance to set done to true.

<aside>
💡 Our example illustrates one of many ways that shared writable state can introduce the kind of intermittent errors for which multithreading is notorious. Next, we look at how to fix our program by locking; however, it’s better to avoid shared state altogether where possible. We see later how asynchronous programming patterns help with this.

</aside>

## **Locking and Thread Safety**

We can fix the previous example by obtaining an exclusive lock while reading and writing to the shared field. C# provides the **lock** statement for just this purpose:

```csharp
class ThreadSafe
{
 static bool _done;
 static readonly object _locker = new object();
 static void Main()
 {
 new Thread (Go).Start();
 Go();
 }
 static void Go()
 {
 lock (_locker)
 {
 if (!_done) { Console.WriteLine ("Done"); _done = true; }
 }
 }
}
```

When two threads simultaneously contend a lock (which can be upon any reference-type object; in this case, _locker), one thread waits, or blocks, until the lock becomes available. In this case, it ensures that only one thread can enter its code block at a time, and “Done” will be printed just once. Code that’s protected in such a manner—from indeterminacy in a multithreaded context—is called thread-safe.

<aside>
💡 Even the act of autoincrementing a variable is not thread-safe: the expression x++ executes on the underlying processor as distinct read-increment-write operations. So, if two threads execute x++ at once outside a lock, the variable can end up get‐ ting incremented once rather than twice (

</aside>

Locking is not a silver bullet for thread safety—it’s easy to forget to lock around accessing a field, and locking can create problems of its own (such as deadlocking).

## Passing Data to a Thread

Sometimes, you’ll want to pass arguments to the thread’s startup method. The easiest way to do this is with a lambda expression that calls the method with the desired arguments:

```csharp
static void Main()
{
 Thread t = new Thread ( () => Print ("Hello from t!") );
 t.Start();
}
static void Print (string message) { Console.WriteLine (message); }
```

With this approach, you can pass in any number of arguments to the method. You can even wrap the entire implementation in a multistatement lambda:

```csharp
new Thread (() =>
{
 Console.WriteLine ("I'm running on another thread!");
 Console.WriteLine ("This is so easy!");
}).Start();
```

An alternative (and less flexible) technique is to pass an argument into Thread’s Start method:

```csharp
static void Main()
{
 Thread t = new Thread (Print);
 t.Start ("Hello from t!");
}
static void Print (object messageObj)
{
 string message = (string) messageObj; // We need to cast here
 Console.WriteLine (message);
}
```

### Lambda expressions and captured variables

As we saw, a lambda expression is the most convenient and powerful way to pass data to a thread. However, you must be careful about accidentally modifying captured variables after starting the thread.

```csharp
for (int i = 0; i < 10; i++)
 new Thread (() => Console.Write (i)).Start();
//The output is nondeterministic! Here’s a typical result:
0223557799
```

The problem is that the i variable refers to the same memory location throughout the loop’s lifetime. Therefore, each thread calls Console.Write on a variable whose value can change as it is running! The solution is to use a temporary variable as follows:

```csharp
for (int i = 0; i < 10; i++)
{
 int temp = i;
 new Thread (() => Console.Write (temp)).Start();
}
```

Variable temp is now local to each loop iteration. Therefore, each thread captures a different memory location and there’s no problem.

## Exception Handling

Any try/catch/finally blocks in effect when a thread is created are of no relevance to the thread when it starts executing. Consider the following program:

```csharp
public static void Main()
{
 try
 {
new Thread (Go).Start();
 }
 catch (Exception ex)
 {
 // We'll never get here!
 Console.WriteLine ("Exception!");
 }
}
static void Go() { throw null; } // Throws a NullReferenceException
```

The try/catch statement in this example is ineffective, and the newly created thread will be encumbered with an unhandled NullReferenceException. This behavior makes sense when you consider that each thread has an independent execution path.

The remedy is to move the exception handler into the Go method:

```csharp
public static void Main()
{
 new Thread (Go).Start();
}
static void Go()
{
 try
 {
 ...
 throw null; // The NullReferenceException will get caught below
 ...
 }
 catch (Exception ex)
 {
 // Typically log the exception, and/or signal another thread
 // that we've come unstuck
 ...
 }
}
```

You need an exception handler on all thread entry methods in production applica‐ tions—just as you do (usually at a higher level, in the execution stack) on your main thread. An unhandled exception causes the whole application to shut down—with an ugly dialog box!

<aside>
💡 In writing such exception handling blocks, rarely would you ignore the error: typically, you’d log the details of the exception. For a client application you might display a dialog box allowing the user to automatically submit those details to your web server. You then might choose to restart the application, because it’s possible that an unexpected exception might leave your program in an invalid state.

</aside>

### Centralized exception handling

In WPF, UWP, and Windows Forms applications, you can subscribe to global exception handling events, Application.DispatcherUnhandledException, and Application.ThreadException, respectively. These fire after an unhandled excep‐ tion in any part of your program that’s called via the message loop (this amounts to all code that runs on the main thread while the Application is active). This is useful as a backstop for logging and reporting bugs (although it won’t fire for unhandled exceptions on non-UI threads that you create). Handling these events prevents the program from shutting down, although you may choose to restart the application to avoid the potential corruption of state that can follow from (or that led to) the unhandled exception.

### Foreground versus Background Threads

By default, threads you create explicitly are foreground threads. Foreground threads keep the application alive for as long as any one of them is running, whereas back‐ ground threads do not. After all foreground threads finish, the application ends, and any background threads still running abruptly terminate.

You can query or change a thread’s background status using its IsBackground property:

```csharp
static void Main (string[] args)
{
 Thread worker = new Thread ( () => Console.ReadLine() );
 if (args.Length > 0) worker.IsBackground = true;
 worker.Start();
}
```

If this program is called with no arguments, the worker thread assumes foreground status and will wait on the ReadLine statement for the user to press Enter. Mean‐ while, the main thread exits, but the application keeps running because a fore‐ ground thread is still alive. On the other hand, if an argument is passed to Main(), the worker is assigned background status, and the program exits almost immediately as the main thread ends (terminating the ReadLine).

When a process terminates in this manner, any finally blocks in the execution stack of background threads are circumvented. If your program employs finally (or using) blocks to perform cleanup work such as deleting temporary files, you can avoid this by explicitly waiting out such background threads upon exiting an application, either by joining the thread, or with a signaling construct

## Thread Priority

A thread’s Priority property determines how much execution time it is allotted rel‐ ative to other active threads in the OS, on the following scale:

```csharp
enum ThreadPriority { Lowest, BelowNormal, Normal, AboveNormal, Highest }
```

This becomes relevant when multiple threads are simultaneously active. You need to take care when elevating a thread’s priority because it can starve other threads. If you want a thread to have higher priority than threads in other processes, you must also elevate the process priority using the Process class in System.Diagnostics:

```csharp
using Process p = Process.GetCurrentProcess();
p.PriorityClass = ProcessPriorityClass.High;
```

This can work well for non-UI processes that do minimal work and need low latency (the ability to respond very quickly) in the work they do. With computehungry applications (particularly those with a user interface), elevating process pri‐ ority can starve other processes, slowing down the entire computer.

## Signaling

Sometimes, you need a thread to wait until receiving notification(s) from other thread(s). This is called signaling. The simplest signaling construct is ManualReset Event. Calling WaitOne on a ManualResetEvent blocks the current thread until another thread “opens” the signal by calling Set. In the following example, we start up a thread that waits on a ManualResetEvent. It remains blocked for two seconds until the main thread signals it:

```csharp
var signal = new ManualResetEvent (false);
new Thread (() =>
{
 Console.WriteLine ("Waiting for signal...");
 signal.WaitOne();
 signal.Dispose();
 Console.WriteLine ("Got signal!");
}).Start();
Thread.Sleep(2000);
signal.Set(); // "Open" the signal
```

After calling Set, the signal remains open; you can close it again by calling Reset

## Threading in Rich Client Applications

In WPF, UWP, and Windows Forms applications, executing long-running opera‐ tions on the main thread makes the application unresponsive because the main thread also processes the message loop that performs rendering and handles key‐ board and mouse events.

A popular approach is to start up “worker” threads for time-consuming operations. The code on a worker thread runs a time-consuming operation and then updates the UI when complete. However, all rich client applications have a threading model whereby UI elements and controls can be accessed only from the thread that created them (typically the main UI thread). Violating this causes either unpredictable behavior, or an exception to be thrown.

Hence when you want to update the UI from a worker thread, you must forward the request to the UI thread (the technical term is marshal). The low-level way to do this is as follows (later, we discuss other solutions which build on these):

- In WPF, call BeginInvoke or Invoke on the element’s Dispatcher object
- In UWP apps, call RunAsync or Invoke on the Dispatcher object
- In Windows Forms, call BeginInvoke or Invoke on the control

All of these methods accept a delegate referencing the method you want to run. BeginInvoke/RunAsync work by enqueuing the delegate to the UI thread’s message queue (the same queue that handles keyboard, mouse, and timer events). Invoke does the same thing, but then blocks until the message has been read and processed by the UI thread. Because of this, Invoke lets you get a return value back from the method. If you don’t need a return value, BeginInvoke/RunAsync are preferable in that they don’t block the caller and don’t introduce the possibility of deadlock

You can imagine that when you call `Application.Run`, the following pseudo-code executes:

```csharp
while (!thisApplication.Ended)
{
 wait for something to appear in message queue
 Got something: what kind of message is it?
 Keyboard/mouse message -> fire an event handler
 User BeginInvoke message -> execute delegate
 User Invoke message -> execute delegate & post result
}
```

To demonstrate, suppose that we have a WPF window that contains a text box called txtMessage, whose content we want a worker thread to update after performing a time-consuming task (which we will simulate by calling `Thread.Sleep`).

```csharp
partial class MyWindow : Window
{
 public MyWindow()
 {
 InitializeComponent();
 new Thread (Work).Start();
 }
 void Work()
 {
 Thread.Sleep (5000); // Simulate time-consuming task
 UpdateMessage ("The answer");
 }
 void UpdateMessage (string message)
 {
 Action action = () => txtMessage.Text = message;
 Dispatcher.BeginInvoke (action);
 }
}
```

Running this results in a responsive window appearing immediately. Five seconds later, it updates the textbox. The code is similar for Windows Forms, except that we call the (Form’s) BeginInvoke method, instead:

```csharp
void UpdateMessage (string message)
 {
 Action action = () => txtMessage.Text = message;
 this.BeginInvoke (action);
 }
```

<aside>
💡 **Multiple UI Threads**
It’s possible to have multiple UI threads if they each own different windows. The main scenario is when you have an application with multiple top-level windows, often called a Single Document Interface (SDI) application, such as Microsoft Word. Each SDI window typically shows itself as a separate “application” on the taskbar and is mostly isolated, functionally, from other SDI windows. By giving each such window its own UI thread, each window can be made more responsive with respect to the others.

</aside>

## Synchronization Contexts

In the `System.ComponentModel` namespace, there’s a class called Synchronization Context, which enables the generalization of thread marshaling

The rich-client APIs for mobile and desktop (UWP, WPF, and Windows Forms) each define and instantiate SynchronizationContext subclasses, which you can obtain via the static property SynchronizationContext.Current (while running on a UI thread). Capturing this property let you later post to UI controls from a worker thread:

```csharp
partial class MyWindow : Window
{
 SynchronizationContext _uiSyncContext;
 public MyWindow()
 {
 InitializeComponent();
 // Capture the synchronization context for the current UI thread:
 _uiSyncContext = SynchronizationContext.Current;
 new Thread (Work).Start();
 }
 void Work()
 {
 Thread.Sleep (5000); // Simulate time-consuming task
 UpdateMessage ("The answer");
 }
 void UpdateMessage (string message)
 {
 // Marshal the delegate to the UI thread:
 _uiSyncContext.Post (_ => txtMessage.Text = message, null);
 }
}
```

This is useful because the same technique works with all rich-client User Interface APIs

## The Thread Pool

Whenever you start a thread, a few hundred microseconds are spent organizing such things as a fresh local variable stack. The thread pool cuts this overhead by having a pool of pre-created recyclable threads. Thread pooling is essential for efficient parallel programming and fine-grained concurrency; it allows short operations to run without being overwhelmed with the overhead of thread startup.

There are a few things to be wary of when using pooled threads

- You cannot set the Name of a pooled thread, making debugging more difficult (although you can attach a description when debugging in Visual Studio’s Threads window).
- Pooled threads are always background threads
- Blocking pooled threads can degrade performance

You are free to change the priority of a pooled thread—it will be restored to normal when released back to the pool.ou can determine whether you’re currently executing on a pooled thread via the property Thread.CurrentThread.IsThreadPoolThread.

### Entering the thread pool

The easiest way to explicitly run something on a pooled thread is to use Task.Run()

```csharp
// Task is in System.Threading.Tasks
Task.Run (() => Console.WriteLine ("Hello from the thread pool"));
```

# Tasks

A thread is a low-level tool for creating concurrency, and as such, it has limitations. In particular

- Although it’s easy to pass data into a thread that you start, there’s no easy way to get a “return value” back from a thread that you Join. You need to set up some kind of shared field. And if the operation throws an exception, catching and propagating that exception is equally painful
- You can’t tell a thread to start something else when it’s finished; instead you must Join it (blocking your own thread in the process)

These limitations discourage fine-grained concurrency; in other words, they make it difficult to compose larger concurrent operations by combining smaller ones (something essential for the asynchronous programming that we look at in following sections). This in turn leads to greater reliance on manual synchronization (locking, signaling, and so on) and the problems that go with it

The direct use of threads also has performance implications. And should you need to run hundreds or thou‐ sands of concurrent I/O-bound operations, a thread-based approach consumes hundreds or thousands of megabytes of memory purely in thread overhead.

The Task class helps with all of these problems. Compared to a thread, a Task is higher-level abstraction—it represents a concurrent operation that might or might not be backed by a thread. Tasks are compositional (you can chain them together through the use of continuations). They can use the thread pool to lessen startup latency, and with a TaskCompletionSource, they can employ a callback approach that avoids threads altogether while waiting on I/O-bound operations.

## Starting a Task

The easiest way to start a Task backed by a thread is with the static method Task.Run (the Task class is in the System.Threading.Tasks namespace). Simply pass in an Action delegate:

```csharp
Task.Run (() => Console.WriteLine ("Foo"));
```

Tasks use pooled threads by default, which are background threads. This means that when the main thread ends, so do any tasks that you create. Hence, to run these examples from a Console application, you must block the main thread after starting the task (for instance, by Waiting the task or by calling Console.ReadLine)

```csharp
static void Main()
{
 Task.Run (() => Console.WriteLine ("Foo"));
 Console.ReadLine();
}
```

You can track a task’s execution status via its Status property

### Wait

Calling Wait on a task blocks until it completes and is the equivalent of calling Join on a thread:

```csharp
Task task = Task.Run (() =>
{
 Thread.Sleep (2000);
 Console.WriteLine ("Foo");
});
Console.WriteLine (task.IsCompleted); // False
task.Wait(); // Blocks until task is complete
```

Wait lets you optionally specify a timeout and a cancellation token to end the wait early

### Long-running tasks

By default, the CLR runs tasks on pooled threads, which is ideal for short-running compute-bound work. For longer-running and blocking operations (such as our preceding example), you can prevent use of a pooled thread as follows:

```csharp
Task task = Task.Factory.StartNew (() => ..., TaskCreationOptions.LongRunning);
```

Running one long-running task on a pooled thread won’t cause trouble; it’s when you run multiple long-running tasks in parallel (particularly ones that block) that performance can suffer. And in that case, there are usually better solutions than `TaskCreationOptions.LongRunning`:

- If the tasks are I/O bound, TaskCompletionSource and asynchronous functions let you implement concurrency with callbacks (continuations) instead of threads.
- If the tasks are compute bound, a producer/consumer queue lets you throttle the concurrency for those tasks, avoiding starvation for other threads and processes

## Returning values

Task has a generic subclass called Task<TResult>, which allows a task to emit a return value. You can obtain a Task<TResult> by calling Task.Run with a Func <TResult> delegate (or a compatible lambda expression) instead of an Action:

```csharp
Task<int> task = Task.Run (() => { Console.WriteLine ("Foo"); return 3; });
// ...
```

You can obtain the result later by querying the Result property. If the task hasn’t yet finished, accessing this property will block the current thread until the task finishes:

```csharp
int result = task.Result; // Blocks if not already finished
Console.WriteLine (result); // 3
```

In the following example, we create a task that uses LINQ to count the number of prime numbers in the first three million (+2) integers:

## Exceptions

Unlike with threads, tasks conveniently propagate exceptions. So, if the code in your task throws an unhandled exception (in other words, if your task faults), that exception is automatically rethrown to whoever calls Wait()—or accesses the Result property of a Task<TResult>

```csharp
// Start a Task that throws a NullReferenceException:
Task task = Task.Run (() => { throw null; });
try
{
 task.Wait();
}
catch (AggregateException aex)
{
 if (aex.InnerException is NullReferenceException)
 Console.WriteLine ("Null!");
 else
 throw;
}
```

## Continuations

A continuation says to a task, “when you’ve finished, continue by doing something else.” A continuation is usually implemented by a callback that executes once upon completion of an operation. There are two ways to attach a continuation to a task. The first is particularly significant because it’s used by C#’s asynchronous functions, as you’ll see soon.

```csharp
Task<int> primeNumberTask = Task.Run (() =>
 Enumerable.Range (2, 3000000).Count (n =>
 Enumerable.Range (2, (int)Math.Sqrt(n)-1).All (i => n % i > 0)));
var awaiter = primeNumberTask.GetAwaiter();
awaiter.OnCompleted (() =>
{
 int result = awaiter.GetResult();
 Console.WriteLine (result); // Writes result
});
```

## TaskCompletionSource

We’ve seen how Task.Run creates a task that runs a delegate on a pooled (or nonpooled) thread. Another way to create a task is with TaskCompletionSource.

TaskCompletionSource lets you create a task out of any operation that starts and finishes some time later. It works by giving you a “slave” task that you manually drive—by indicating when the operation finishes or faults. This is ideal for I/Obound work: you get all the benefits of tasks (with their ability to propagate return values, exceptions, and continuations) without blocking a thread for the duration of the operation.

To use TaskCompletionSource, you simply instantiate the class. It exposes a Task property that returns a task upon which you can wait and attach continuations— just as with any other task. The task, however, is controlled entirely by the TaskCompletionSource object via the following methods

```csharp
public class TaskCompletionSource<TResult>
{
 public void SetResult (TResult result);
 public void SetException (Exception exception);
 public void SetCanceled();
 public bool TrySetResult (TResult result);
 public bool TrySetException (Exception exception);
 public bool TrySetCanceled();
 public bool TrySetCanceled (CancellationToken cancellationToken);
 ...
}
```

```csharp
var tcs = new TaskCompletionSource<int>();
new Thread (() => { Thread.Sleep (5000); tcs.SetResult (42); })
 { IsBackground = true }
 .Start();
Task<int> task = tcs.Task; // Our "slave" task.
Console.WriteLine (task.Result); // 42
```

With TaskCompletionSource, we can write our own Run method

```csharp
void Main()
{
    Task<int> task = Run (() => { Thread.Sleep (5000); return 42; });
    task.Result.Dump();
}

Task<TResult> Run<TResult> (Func<TResult> function)
{
    var tcs = new TaskCompletionSource<TResult>();
    new Thread (() => 
    {
        try { tcs.SetResult (function()); }
        catch (Exception ex) { tcs.SetException (ex); }
    }).Start();
    return tcs.Task;
}
```

Now we can write our general-purpose Delay method

```csharp
void Main()
{
    Delay (5000).GetAwaiter().OnCompleted (() => Console.WriteLine (42));
}

Task Delay (int milliseconds)
{
    var tcs = new TaskCompletionSource<object>();
    var timer = new System.Timers.Timer (milliseconds) { AutoReset = false };
    timer.Elapsed += delegate { timer.Dispose(); tcs.SetResult (null); };
    timer.Start();
    return tcs.Task;
}
```

## Task.Delay

The Delay method that we just wrote is sufficiently useful that it’s available as a static method on the Task class

```csharp
Task.Delay (5000).GetAwaiter().OnCompleted (() => Console.WriteLine (42));
```

Task.Delay is the asynchronous equivalent of Thread.Sleep.

# Principles of Asynchrony

## Synchronous versus Asynchronous Operations

A synchronous operation does its work before returning to the caller. 

An asynchronous operation can do (most or all of) its work after returning to the caller.

The majority of methods that you write and call are synchronous. An example is List<T>.Add, or Console.WriteLine, or Thread.Sleep. Asynchronous methods are less common and initiate concurrency, because work continues in parallel to the caller. Asynchronous methods typically return quickly (or immediately) to the caller; thus, they are also called nonblocking methods.

Most of the asynchronous methods that we’ve seen so far can be described as general-purpose methods

- Thread.Start
- Task.Run
- Methods that attach continuations to tasks

## What Is Asynchronous Programming?

The principle of asynchronous programming is that you write long-running (or potentially long-running) functions asynchronously. This is in contrast to the conventional approach of writing long-running functions synchronously, and then calling those functions from a new thread or task to introduce concurrency as required.

The difference with the asynchronous approach is that concurrency is initiated inside the long-running function rather than from outside the function. This has two benefits

- I/O-bound concurrency can be implemented without tying up threads (as we demonstrate in “TaskCompletionSource” on page 598), improving scalability and efficiency.
- Rich-client applications end up with less code on worker threads, simplifying thread safety

This, in turn, leads to two distinct uses for asynchronous programming. The first is writing (typically server-side) applications that deal efficiently with a lot of concur‐ rent I/O. The challenge here is not thread safety (because there’s usually minimal shared state) but thread efficiency; in particular, not consuming a thread per net‐ work request. So, in this context, it’s only I/O-bound operations that benefit from asynchrony.

The second use is to simplify thread safety in rich-client applications. This is particularly relevant as a program grows in size, because to deal with complexity, we typically refactor larger methods into smaller ones, resulting in chains of methods that call one another (call graphs).

With a traditional synchronous call graph, if any operation within the graph is longrunning, we must run the entire call graph on a worker thread to maintain a responsive UI. Hence, we end up with a single concurrent operation that spans many methods (coarse-grained concurrency), and this requires considering thread safety for every method in the graph.

With an asynchronous call graph, we need not start a thread until it’s actually needed, typically low in the graph (or not at all in the case of I/O-bound operations). All other methods can run entirely on the UI thread, with much-simplified thread safety. This results in fine-grained concurrency—a sequence of small concur‐ rent operations, between which execution bounces to the UI thread.

To benefit from this, both I/O- and compute-bound operations need to be written asynchronously; a good rule of thumb is to include anything that might take longer than 50 ms.

# Obsolete Patterns

## Event-Based Asynchronous Pattern

### BackgroundWorker

BackgroundWorker in System.ComponentModel is a general-purpose implementa‐ tion of the EAP. It allows rich-client apps to start a worker thread and report completion and percentage-based progress without needing to explicitly capture synchronization context. Here’s an example:

```csharp
var worker = new BackgroundWorker { WorkerSupportsCancellation = true };
worker.DoWork += (sender, args) =>
{ // This runs on a worker thread
 if (args.Cancel) return;
 Thread.Sleep(1000);
 args.Result = 123;
};
worker.RunWorkerCompleted += (sender, args) =>
{ // Runs on UI thread
 // We can safely update UI controls here...
 if (args.Cancelled)
 Console.WriteLine ("Cancelled");
 else if (args.Error != null)
 Console.WriteLine ("Error: " + args.Error.Message);
 else
 Console.WriteLine ("Result is: " + args.Result);
};
worker.RunWorkerAsync(); // Captures sync context and starts operation
```

RunWorkerAsync starts the operation, firing the DoWork event on a pooled worker thread. It also captures the synchronization context, and when the operation completes (or faults), the RunWorkerCompleted event is invoked through that synchroni‐ zation context (like a continuation).# Chapter 15 : Streams and I/O

# Stream Architecture

The .NET stream architecture centers on three concepts: backing stores, decorators, and adapters

A backing store is the endpoint that makes input and output useful, such as a file or network connection. Precisely, it is either or both of the following

- A source from which bytes can be sequentially read
- A destination to which bytes can be sequentially written

A backing store is of no use, though, unless exposed to the programmer. A Stream is the standard .NET class for this purpose; it exposes a standard set of methods for reading, writing, and positioning. Unlike an array, for which all the backing data exists in memory at once, a stream deals with data serially—either one byte at a time or in blocks of a manageable size. Hence, a stream can use a small, fixed amount of memory regardless of the size of its backing store.


Backing store streams

- These are hardwired to a particular type of backing store, such as FileStream or NetworkStream.

Decorator streams

- These feed off another stream, transforming the data in some way, such as DeflateStream or CryptoStream.

Decorator streams have the following architectural benefit

- They liberate backing store streams from needing to implement such features as compression and encryption themselves.
- Streams don’t suffer a change of interface when decorated.
- You connect decorators at runtime.
- You can chain decorators together (e.g., a compressor followed by an encryptor).

Both backing store and decorator streams deal exclusively in bytes. Although this is flexible and efficient, applications often work at higher levels such as text or XML. Adapters bridge this gap by wrapping a stream in a class with specialized methods typed to a particular format. For example, a text reader exposes a ReadLine method; an XML writer exposes a WriteAttributes method.

To summarize, backing store streams provide the raw data; decorator streams provide transparent binary transformations such as encryption; adapters offer typed methods for dealing in higher-level types such as strings and XML.

# Using Streams

The abstract Stream class is the base for all streams. It defines methods and proper‐ ties for three fundamental operations: reading, writing, and seeking, as well as for administrative tasks such as closing, flushing, and configuring timeouts

There are also asynchronous versions of the Read and Write methods, both of which return Tasks and optionally accept a cancellation token, and overloads that work with Span<T> and Memory<T> types

```csharp
using System;
using System.IO;
class Program
{
 static void Main()
 {
 // Create a file called test.txt in the current directory:
 using (Stream s = new FileStream ("test.txt", FileMode.Create))
 {
 Console.WriteLine (s.CanRead); // True
 Console.WriteLine (s.CanWrite); // True
 Console.WriteLine (s.CanSeek); // True
 s.WriteByte (101);
 s.WriteByte (102);
 byte[] block = { 1, 2, 3, 4, 5 };
 s.Write (block, 0, block.Length); // Write block of 5 bytes
 Console.WriteLine (s.Length); // 7
 Console.WriteLine (s.Position); // 7
 **s.Position = 0; // Move back to the start**
 Console.WriteLine (s.ReadByte()); // 101
 Console.WriteLine (s.ReadByte()); // 102
 // Read from the stream back into the block array:
 Console.WriteLine (s.Read (block, 0, block.Length)); // 5
 // Assuming the last Read returned 5, we'll be at
 // the end of the file, so Read will now return 0:
 Console.WriteLine (s.Read (block, 0, block.Length)); // 0
 }
 }
}
```

## Reading and Writing

A stream can support reading, writing, or both. If CanWrite returns false, the stream is read-only; if CanRead returns false, the stream is write-only

Read receives a block of data from the stream into an array. It returns the number of bytes received, which is always either less than or equal to the count argument. If it’s less than count, it means that either the end of the stream has been reached or the stream is giving you the data in smaller chunks (as is often the case with network streams). In either case, the balance of bytes in the array will remain unwritten, their previous values preserved.

The Read method could read anywhere from 1 to 1,000 bytes, leaving the balance of the stream unread.

Here’s the correct way to read a 1,000-byte stream:

```csharp
byte[] data = new byte [1000];
// bytesRead will always end up at 1000, unless the stream is
// itself smaller in length:
int bytesRead = 0;
int chunkSize = 1;
while (bytesRead < data.Length && chunkSize > 0)
 bytesRead +=
 chunkSize = s.Read (data, bytesRead, data.Length - bytesRead);
```

Fortunately, the BinaryReader type provides a simpler way to achieve the same result

```csharp
byte[] data = new BinaryReader (s).ReadBytes (1000);
```

## Seeking

A stream is seekable if CanSeek returns true. With a seekable stream (such as a file stream), you can query or modify its Length (by calling SetLength) and at any time change the Position at which you’re reading or writing. The Position property is relative to the beginning of the stream; the Seek method, however, allows you to move relative to the current position or the end of the stream.

With a nonseekable stream (such as an encryption stream), the only way to deter‐ mine its length is to read it completely through. Furthermore, if you need to reread a previous section, you must close the stream and start afresh with a new one.

## Closing and Flushing

Streams must be disposed after use to release underlying resources such as file and socket handles. A simple way to guarantee this is by instantiating streams within using blocks.

- Dispose and Close are identical in function.
- Disposing or closing a stream repeatedly causes no error.

## Timeouts

A stream supports read and write timeouts if CanTimeout returns true. Network streams support timeouts; file and memory streams do not. For streams that sup‐ port timeouts, the ReadTimeout and WriteTimeout properties determine the desired timeout in milliseconds, where 0 means no timeout. The Read and Write methods indicate that a timeout has occurred by throwing an exception.

## Thread Safety

As a rule, streams are not thread-safe, meaning that two threads cannot concurrently read or write to the same stream without possible error. The Stream class offers a simple workaround via the static Synchronized method. This method accepts a stream of any type and returns a thread-safe wrapper. The wrapper works by obtaining an exclusive lock around each read, write, or seek, ensuring that only one thread can perform such an operation at a time. In practice, this allows multiple threads to simultaneously append data to the same stream—other kinds of activities (such as concurrent reading) require additional locking to ensure that each thread accesses the desired portion of the stream.

## File Streams

The simplest way to instantiate a FileStream is to use one of the following static façade methods on the File class

```csharp
FileStream fs1 = File.OpenRead ("readme.bin"); // Read-only
FileStream fs2 = File.OpenWrite ("writeme.tmp"); // Write-only
FileStream fs3 = File.Create ("readwrite.tmp"); // Read/write
```

You can also directly instantiate a FileStream. Its constructors provide access to every feature, allowing you to specify a filename or low-level file handle, file creation and access modes, and options for sharing, buffering, and security

```csharp
sing var fs = new FileStream ("readwrite.tmp", FileMode.Open);
```

### Shortcut Methods on the File Class

The following static methods read an entire file into memory in one step

- File.ReadAllText (returns a string)
- File.ReadAllLines (returns an array of strings)
- File.ReadAllBytes (returns a byte array)

The following static methods write an entire file in one step:

- File.WriteAllText
- File.WriteAllLines
- File.WriteAllBytes
- File.AppendAllText (great for appending to a log file)

There’s also a static method called File.ReadLines: this is like ReadAllLines except that it returns a lazily evaluated IEnumerable<string>. This is more efficient because it doesn’t load the entire file into memory at once. LINQ is ideal for consuming the results: the following calculates the number of lines greater than 80 characters in length:

```csharp
int longLines = File.ReadLines ("filePath")
 .Count (l => l.Length > 80);
```

### Specifying a filename

A filename can be either absolute (e.g., c:\temp\test.txt—or in Unix, /tmp/test.txt) or relative to the current directory (e.g., test.txt or temp\test.txt). You can access or change the current directory via the static `Environment.CurrentDirectory` property.

`AppDomain.CurrentDomain.BaseDirectory` returns the application base directory, which in normal cases is the folder containing the program’s executable. To specify a filename relative to this directory, you can call Path.Combine:

```csharp
string baseFolder = AppDomain.CurrentDomain.BaseDirectory;
string logoPath = Path.Combine (baseFolder, "logo.jpg");
Console.WriteLine (File.Exists (logoPath));
```

You can read and write across a Windows network via a Universal Naming Conven‐ tion (UNC) path, such as \\JoesPC\PicShare\pic.jpg or \\10.1.1.2\PicShare\pic.jpg.

### Specifying a FileMode

All of FileStream’s constructors that accept a filename also require a FileMode enum argument.

File.Create and FileMode.Create will throw an exception if used on hidden files. To overwrite a hidden file, you must delete and re-create it:

```csharp
File.Delete ("hidden.txt");
using var file = File.Create ("hidden.txt");
...
```

### Advanced FileStream features

- A FileShare enum describing how much access to grant other processes want‐ ing to dip into the same file before you’ve finished (None, Read [default], Read Write, or Write).
- The size, in bytes, of the internal buffer (default is currently 4 KB).
- A flag indicating whether to defer to the operating system for asynchronous I/O
- A FileOptions flags enum for requesting operating system encryption (Encrypted), automatic deletion upon closure for temporary files (DeleteOn Close), and optimization hints (RandomAccess and SequentialScan). There is also a WriteThrough flag that requests that the OS disable write-behind caching; this is for transactional files or logs. Flags not supported by the underlying OS are silently ignored.

## MemoryStream

MemoryStream uses an array as a backing store. This partly defeats the purpose of having a stream because the entire backing store must reside in memory at once. MemoryStream is still useful when you need random access to a nonseekable stream. If you know the source stream will be of a manageable size, you can copy it into a MemoryStream as follows:

```csharp
var ms = new MemoryStream();
sourceStream.CopyTo (ms);
```

You can convert a MemoryStream to a byte array by calling ToArray. The GetBuffer method does the same job more efficiently by returning a direct reference to the underlying storage array

## PipeStream

PipeStream provides a simple means by which one process can communicate with another through the operating system’s pipes protocol. There are two kinds of pipe:

Anonymous pipe (faster)

- Allows one-way communication between a parent and child process on the same computer.

Named pipe (more flexible)

- Allows two-way communication between arbitrary processes on the same computer or different computers across a network.

A pipe is good for interprocess communication (IPC) on a single computer

# Stream Adapters

A Stream deals only in bytes; to read or write data types such as strings, integers, or XML elements, you must plug in an adapter. Here’s what .NET Core provides

- Text adapters (for string and character data)
    - TextReader, TextWriter
    - StreamReader, StreamWriter
    - StringReader, StringWriter
- Binary adapters (for primitive types such as int, bool, string, and float)
    - BinaryReader, BinaryWriter
- XML adapters
    - XmlReader, XmlWriter

## Text Adapters

TextReader and TextWriter are the abstract base classes for adapters that deal exclusively with characters and strings. Each has two general-purpose implementations in .NET Core

- StreamReader/StreamWriter
- StringReader/StringWriter

Peek returns the next character in the stream without advancing the position. Both Peek and the zero-argument version of Read return −1 if at the end of the stream;

otherwise, they return an integer that can be cast directly to a char. The overload of Read that accepts a char[] buffer is identical in functionality to the ReadBlock method.

<aside>
💡 `Environment.NewLine` returns the newline sequence for the current OS. On Windows, this is "\r\n" (think “ReturN”) and is loosely modeled on a mechanical typewriter: a CR (character 13) fol‐ lowed by an LF (character 10).

</aside>

### StreamReader and StreamWriter

```csharp
using (FileStream fs = File.Create ("test.txt"))
using (TextWriter writer = new StreamWriter (fs))
{
 writer.WriteLine ("Line1");
 writer.WriteLine ("Line2");
}
using (FileStream fs = File.OpenRead ("test.txt"))
using (TextReader reader = new StreamReader (fs))
{
 Console.WriteLine (reader.ReadLine()); // Line1
 Console.WriteLine (reader.ReadLine()); // Line2
}
```

Because text adapters are so often coupled with files, the File class provides the static methods CreateText, AppendText, and OpenText to shortcut the process

```csharp
using (TextWriter writer = File.CreateText ("test.txt"))
{
 writer.WriteLine ("Line1");
 writer.WriteLine ("Line2");
}
using (TextWriter writer = File.AppendText ("test.txt"))
 writer.WriteLine ("Line3");
using (TextReader reader = File.OpenText ("test.txt"))
 while (reader.Peek() > -1)
 Console.WriteLine (reader.ReadLine()); // Line1
 // Line2
 // Line3
```

This also illustrates how to test for the end of a file (viz. reader.Peek()). Another option is to read until reader.ReadLine returns null.

### Character encodings

TextReader and TextWriter are by themselves just abstract classes with no connec‐ tion to a stream or backing store. The StreamReader and StreamWriter types, how‐ ever, are connected to an underlying byte-oriented stream, so they must convert between characters and bytes. They do so through an Encoding class from the System.Text namespace, which you choose when constructing the StreamReader or StreamWriter. If you choose none, the default UTF-8 encoding is used.

## Binary Adapters

BinaryReader and BinaryWriter read and write native data types: bool, byte, char, decimal, float, double, short, int, long, sbyte, ushort, uint, and ulong, as well as strings and arrays of the primitive data types.

Unlike StreamReader and StreamWriter, binary adapters store primitive data types efficiently because they are represented in memory. So, an int uses four bytes; a double uses eight bytes. Strings are written through a text encoding (as with Stream Reader and StreamWriter) but are length-prefixed in order to make it possible to read back a series of strings without needing special delimiters.

```csharp
//Imagine that we have a simple type, defined as follows:
public class Person
{
 public string Name;
 public int Age;
 public double Height;
}
//We can add the following methods to Person to save/load its data to/from a stream
using binary adapters:

public void SaveData (Stream s)
{
 var w = new BinaryWriter (s);
 w.Write (Name);
 w.Write (Age);
 w.Write (Height);
 w.Flush(); // Ensure the BinaryWriter buffer is cleared.
 // We won't dispose/close it, so more data
} // can be written to the stream.

public void LoadData (Stream s)
{
 var r = new BinaryReader (s);
 Name = r.ReadString();
 Age = r.ReadInt32();
 Height = r.ReadDouble();
}
```

## Closing and Disposing Stream Adapters

You have four choices in tearing down stream adapters:

- Close the adapter only
- Close the adapter and then close the stream
- (For writers) Flush the adapter and then close the stream
- (For readers) Close just the stream

Options 1 and 2 are semantically identical because closing an adapter automatically closes the underlying stream. Whenever you nest using statements, you’re implicitly taking option 2:

```csharp
using (FileStream fs = File.Create ("test.txt"))
using (TextWriter writer = new StreamWriter (fs))
 writer.WriteLine ("Line");
```

Because the nest disposes from the inside out, the adapter is first closed, and then the stream. Furthermore, if an exception is thrown within the adapter’s constructor, the stream still closes. It’s hard to go wrong with nested using statements!

# Compression Streams

Two general-purpose compression streams are provided in the [System.IO](http://system.io/) .Compression namespace: DeflateStream and GZipStream. Both use a popular compression algorithm similar to that of the ZIP format. They differ in that GZip Stream writes an additional protocol at the start and end—including a CRC to detect errors. GZipStream also conforms to a standard recognized by other software.

NET Core also includes BrotliStream, which implements the Brotli compression algorithm. BrotliStream is more than 10 times slower than DeflateStream and GZipStream but achieves a better compression ratio. (The performance hit applies only to compression—decompression performs very well.)

All three streams allow reading and writing, with the following provisos:

- You always write to the stream when compressing
- You always read from the stream when decompressing.

DeflateStream, GZipStream, and BrotliStream are decorators; they compress or decompress data from another stream that you supply in construction. In the following example, we compress and decompress a series of bytes using a File Stream as the backing store:

```csharp
using (Stream s = File.Create ("compressed.bin"))
using (Stream ds = new DeflateStream (s, CompressionMode.Compress))
 for (byte i = 0; i < 100; i++)
 ds.WriteByte (i);

using (Stream s = File.OpenRead ("compressed.bin"))
using (Stream ds = new DeflateStream (s, CompressionMode.Decompress))
 for (byte i = 0; i < 100; i++)
 Console.WriteLine (ds.ReadByte()); // Writes 0 to 99
```

# File and Directory Operations

The `System.IO` namespace provides a set of types for performing “utility” file and directory operations, such as copying and moving, creating directories, and setting file attributes and permissions. For most features, you can choose between either of two classes, one offering static methods and the other instance methods:

- Static classes : File and Directory
- Instance-method classes (constructed with a file or directory name)
    - FileInfo and DirectoryInfo

Additionally, there’s a static class called Path. This does nothing to files or directories; instead, it provides string manipulation methods for filenames and directory paths. Path also assists with temporary files.

## The File Class

File is a static class whose methods all accept a filename. The filename can be either relative to the current directory or fully qualified with a directory. Here are its meth‐ ods (all public and static):

```csharp
bool Exists (string path); // Returns true if the file is present
void Delete (string path);
void Copy (string sourceFileName, string destFileName);
void Move (string sourceFileName, string destFileName);
void Replace (string sourceFileName, string destinationFileName,
 string destinationBackupFileName);
FileAttributes GetAttributes (string path);
void SetAttributes (string path, FileAttributes fileAttributes);
void Decrypt (string path);
void Encrypt (string path);
DateTime GetCreationTime (string path); // UTC versions are
DateTime GetLastAccessTime (string path); // also provided.
DateTime GetLastWriteTime (string path);
void SetCreationTime (string path, DateTime creationTime);
void SetLastAccessTime (string path, DateTime lastAccessTime);
void SetLastWriteTime (string path, DateTime lastWriteTime);
FileSecurity GetAccessControl (string path);
FileSecurity GetAccessControl (string path,
 AccessControlSections includeSections);
void SetAccessControl (string path, FileSecurity fileSecurity);
```

Move throws an exception if the destination file already exists; Replace does not. Both methods allow the file to be renamed as well as moved to another directory.

Delete throws an UnauthorizedAccessException if the file is marked read-only; you can tell this in advance by calling GetAttributes. It also throws that exception if the OS denies delete permission for that file to your process. Here are all the members of the FileAttribute enum that GetAttributes returns:

## The Directory Class

The static Directory class provides a set of methods analogous to those in the File class—for checking whether a directory exists (Exists), moving a directory (Move), deleting a directory (Delete), getting/setting times of creation or last access, and getting/setting security permissions. Furthermore, Directory exposes the following static methods:

```csharp
string GetCurrentDirectory ();
void SetCurrentDirectory (string path);
DirectoryInfo CreateDirectory (string path);
DirectoryInfo GetParent (string path);
string GetDirectoryRoot (string path);
string[] GetLogicalDrives(); // Gets mount points on Unix
// The following methods all return full paths:

string[] GetFiles (string path);
string[] GetDirectories (string path);
string[] GetFileSystemEntries (string path);
IEnumerable<string> EnumerateFiles (string path);
IEnumerable<string> EnumerateDirectories (string path);
IEnumerable<string> EnumerateFileSystemEntries (string path);
```

Here’s how to create a directory if it doesn’t already exist:

```csharp
Here’s how to create a directory if it doesn’t already exist:
if (!Directory.Exists (@"d:\test"))
 Directory.CreateDirectory (@"d:\test");
```

## FileInfo and DirectoryInfo

The static methods on File and Directory are convenient for executing a single file or directory operation. If you need to call a series of methods in a row, the FileInfo and DirectoryInfo classes provide an object model that makes the job easier.

FileInfo offers most of the File’s static methods in instance form—with some additional properties such as Extension, Length, IsReadOnly, and Directory—for returning a DirectoryInfo object:

```csharp
static string TestDirectory =>
 RuntimeInformation.IsOSPlatform (OSPlatform.Windows)
 ? @"C:\Temp"
 : "/tmp";
Directory.CreateDirectory (TestDirectory);
FileInfo fi = new FileInfo (Path.Combine (TestDirectory, "FileInfo.txt"));
Console.WriteLine (fi.Exists); // false
using (TextWriter w = fi.CreateText())
 w.Write ("Some text");
Console.WriteLine (fi.Exists); // false (still)
fi.Refresh();
Console.WriteLine (fi.Exists); // true
Console.WriteLine (fi.Name); // FileInfo.txt
Console.WriteLine (fi.FullName); // c:\temp\FileInfo.txt (Windows)
 // /tmp/FileInfo.txt (Unix)
Console.WriteLine (fi.DirectoryName); // c:\temp (Windows)
 // /tmp (Unix)
Console.WriteLine (fi.Directory.Name); // temp
Console.WriteLine (fi.Extension); // .txt
Console.WriteLine (fi.Length); // 9
fi.Encrypt();
fi.Attributes ^= FileAttributes.Hidden; // (Toggle hidden flag)
fi.IsReadOnly = true;
Console.WriteLine (fi.Attributes); // ReadOnly,Archive,Hidden,Encrypted
Console.WriteLine (fi.CreationTime); // 3/09/2019 1:24:05 PM
fi.MoveTo (Path.Combine (TestDirectory, "FileInfoX.txt"));
DirectoryInfo di = fi.Directory;
Console.WriteLine (di.Name); // temp or tmp
Console.WriteLine (di.FullName); // c:\temp or /tmp
Console.WriteLine (di.Parent.FullName); // c:\ or /
di.CreateSubdirectory ("SubFolder");
```

Here’s how to use DirectoryInfo to enumerate files and subdirectories

```csharp
DirectoryInfo di = new DirectoryInfo (@"e:\photos");
foreach (FileInfo fi in di.GetFiles ("*.jpg"))
 Console.WriteLine (fi.Name);
foreach (DirectoryInfo subDir in di.GetDirectories())
 Console.WriteLine (subDir.FullName);
```

## Path

The static Path class defines methods and fields for working with paths and filenames.

Assuming this setup code

```csharp
string dir = @"c:\mydir"; // or /mydir
string file = "myfile.txt";
string path = @"c:\mydir\myfile.txt"; // or /mydir/myfile.txt
Directory.SetCurrentDirectory (@"k:\demo"); // or /demo
```

## Special Folders

One thing missing from Path and Directory is a means to locate folders such as My Documents, Program Files, Application Data, and so on. This is provided instead by the `GetFolderPath` method in the `System.Environment` class:

```csharp
string myDocPath = Environment.GetFolderPath
 (Environment.SpecialFolder.MyDocuments);
```

Environment.SpecialFolder is an enum whose values encompass all special direc‐ tories in Windows, such as AdminTools, ApplicationData, Fonts, History, SendTo, StartMenu, and so on. Everything is covered here except the .NET Core directory, which you can obtain as follows:

```csharp
System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory()
```

Of particular value on Windows systems is ApplicationData: this is where you can store settings that travel with a user across a network (if roaming profiles are enabled on the network domain) and LocalApplicationData, which is for non‐ roaming data (specific to the logged-in user) and CommonApplicationData, which is shared by every user of the computer. Writing application data to these folders is considered preferable to using the Windows Registry. The standard protocol for storing data in these folders is to create a subdirectory with the name of your application:

```csharp
string localAppDataPath = Path.Combine (
 Environment.GetFolderPath (Environment.SpecialFolder.ApplicationData),
 "MyCoolApplication");
if (!Directory.Exists (localAppDataPath))
 Directory.CreateDirectory (localAppDataPath);
```

## Querying Volume Information

You can query the drives on a computer with the DriveInfo class:

```csharp
DriveInfo c = new DriveInfo ("C"); // Query the C: drive
 // On Unix: /
long totalSize = c.TotalSize; // Size in bytes
long freeBytes = c.TotalFreeSpace; // Ignores disk quotas
long freeToMe = c.AvailableFreeSpace; // Takes quotas into account
foreach (DriveInfo d in DriveInfo.GetDrives()) // All defined drives
 // On Unix: mount points
{
 Console.WriteLine (d.Name); // C:\
 Console.WriteLine (d.DriveType); // Fixed
 Console.WriteLine (d.RootDirectory); // C:\
 if (d.IsReady) // If the drive is not ready, the following two
 // properties will throw exceptions:
 {
 Console.WriteLine (d.VolumeLabel); // The Sea Drive
 Console.WriteLine (d.DriveFormat); // NTFS
 }
}
```

## Catching Filesystem Events

The FileSystemWatcher class lets you monitor a directory (and optionally, subdirectories) for activity. FileSystemWatcher has events that fire when files or subdirectories are created, modified, renamed, and deleted, as well as when their attributes change. These events fire regardless of the user or process performing the change. Here’s an example:

```csharp
static void Main() => Watch (TestDirectory, "*.txt", true);
static void Watch (string path, string filter, bool includeSubDirs)
{
using (var watcher = new FileSystemWatcher (path, filter))
 {
 watcher.Created += FileCreatedChangedDeleted;
 watcher.Changed += FileCreatedChangedDeleted;
 watcher.Deleted += FileCreatedChangedDeleted;
 watcher.Renamed += FileRenamed;
 watcher.Error += FileError;
 watcher.IncludeSubdirectories = includeSubDirs;
 watcher.EnableRaisingEvents = true;
 Console.WriteLine ("Listening for events - press <enter> to end");
 Console.ReadLine();
 }
 // Disposing the FileSystemWatcher stops further events from firing.
}
static void FileCreatedChangedDeleted (object o, FileSystemEventArgs e)
 => Console.WriteLine ("File {0} has been {1}", e.FullPath, e.ChangeType);
static void FileRenamed (object o, RenamedEventArgs e)
 => Console.WriteLine ("Renamed: {0}->{1}", e.OldFullPath, e.FullPath);
static void FileError (object o, ErrorEventArgs e)
 => Console.WriteLine ("Error: " + e.GetException().Message);
static string TestDirectory =>
 RuntimeInformation.IsOSPlatform (OSPlatform.Windows)
 ? @"C:\Temp"
 : "/tmp";
```

# OS Security

All applications are subject to OS restrictions, based on the user’s login privileges. These restrictions affect file I/O as well as other capabilities, such as access to the Windows Registry.

In Windows and Unix, there are two types of accounts

- An administrative/superuser account that imposes no restrictions in accessing the local computer
- A limited permissions account that restricts administrative functions and visi‐ bility of other users’ data

On Windows, a feature called User Account Control (UAC) means that administra‐ tors receive two tokens or “hats” when logging in: an administrative hat and an ordinary user hat. By default, programs run wearing the ordinary user hat—with restricted permissions—unless the program requests administrative elevation. The user must then approve the request in the dialog box that’s presented.

By default, your application will run with restricted user privileges. This means that you must either:

- Write your application such that it can run without administrative privileges
- Demand administrative elevation in the application manifest (Windows only), or detect the lack of required privileges and alert the user to restart the applica‐ tion as an administrator/super-user.

The first option is safer and more convenient to the user. Designing your program to run without administrative privileges is easy in most cases

You can find out whether you’re running under an administrative account, as follows:

```csharp
[DllImport("libc")]
public static extern uint getuid();
static bool IsRunningAsAdmin()
{
 if (RuntimeInformation.IsOSPlatform (OSPlatform.Windows))
 {
 using var identity = WindowsIdentity.GetCurrent();
 var principal = new WindowsPrincipal (identity);
 return principal.IsInRole (WindowsBuiltInRole.Administrator);
 }
 return getuid() == 0;
}
```

### Running in a Standard User Account

Here are the key things that you cannot do in a standard user account

- Write to the following directories:
    - The OS folder (typically \Windows or /bin, /sbin, ...) and subdirectories
    - The program files folder (\Program Files or /usr/bin, /opt) and subdirectories
    - The root of the OS drive (e.g., C:\ or /)
- Write to the HKEY_LOCAL_MACHINE branch of the Registry (Windows)
- Read performance monitoring (WMI) data (Windows)

If you’re refused access to anything as a result of OS security, the CLR detects the failure and throws an UnauthorizedAccessException (rather than failing silently).

The recommended location for user documents is SpecialFolder.MyDocuments:

```csharp
string docsFolder = Environment.GetFolderPath
 (Environment.SpecialFolder.MyDocuments);
string path = Path.Combine (docsFolder, "test.txt");
```# Chapter 17 : Serialization

This chapter introduces serialization and deserialization, the mechanism by which objects can be represented in a flat-text or binary form. Unless otherwise stated, the types in this chapter all exist in the following namespaces

```csharp
System.Runtime.Serialization
System.Xml.Serialization
System.Text.Json
```

# Serialization Concepts

Serialization is the act of taking an in-memory object or object graph (set of objects that reference one another) and flattening it into a stream of bytes, XML, JSON, or a similar representation that can be stored or transmitted.

Deserialization works in reverse, taking a data stream and reconstituting it into an in-memory object or object graph.

Serialization and deserialization are typically used to do the following

- Transmit objects across a network or application boundary
- Store representations of objects within a file or database

Another, less common use is to deep-clone objects. You also can use the data con‐ tract and XML serialization engines as general-purpose tools for loading and saving XML files of a known structure, whereas the JSON serializer can do the same for JSON files.

## Serialization Engines

There are four serialization engines in .NET Core:

- XmlSerializer (XML)
- JsonSerializer (JSON)
- The (somewhat redundant) data contract serializer (XML and JSON)
- The binary serializer (binary)

If you’re serializing to XML, you can choose between XmlSerializer and the data contract serializer. XmlSerializer offers greater flexibility on how the XML is structured, whereas the data contract serializer has the unique ability to preserve shared object references.

If you’re serializing to JSON, you also have a choice. JsonSerializer offers the best performance, whereas the data contract serializer has a few extra features due to its longer heritage. However, **if you need extra features, a better choice is likely to be the third-party [`Json.NET`](http://json.net/) library.**

And if you don’t care about the format, the binary serialization engine is the most powerful and easiest to use. The output, however, is not human-readable and it’s less version-tolerant than the other serializers.

## XmlSerializer

The XML serialization engine can produce only XML, and it is less powerful than the binary and data contract serializers in saving and restoring a complex object graph (it cannot restore shared object references). It’s the most flexible of the four, however, in following an arbitrary output structure. For instance, you can choose whether properties are serialized to elements or attributes and the handling of a col‐ lection’s outer element. The XML engine also provides excellent version tolerance. XmlSerializer was used by the legacy ASMX web services.

## JsonSerializer

The JSON serializer is fast and efficient, and was introduced relatively recently to .NET Core. It also offers good version tolerance and allows the use of custom converters for flexibility. JsonSerializer is used by [ASP.NET](http://asp.net/) Core 3, removing the dependency on [Json.NET](http://json.net/), though it is straightforward to opt back in to [Json.NET](http://json.net/) should its features be required.

## The binary serializer

The binary serialization engine is easy to use, highly automatic, and well supported throughout .NET Core 3 (and even more so in .NET Framework). Quite often, a single attribute is all that’s required to make a complex type fully serializable. The binary serializer is also faster than the data contract serializer when full type fidelity is needed. However, it tightly couples a type’s internal structure to the format of the serialized data, resulting in poor version tolerance (although it can tolerate the sim‐ ple addition of a field). The binary engine emits only binary data;

## Formatters

The output of the data contract and binary serializers is shaped by a pluggable formatter. The role of a formatter is the same with both serialization engines, although they use completely different classes to do the job.

A formatter shapes the final presentation to suit a particular medium or context of serialization. In .NET Core, the data contract serializer lets you choose between XML and JSON formatters, and in .NET Framework you can also choose a binary formatter. A binary formatter is designed to work in a context for which an arbi‐ trary stream of bytes will do—typically a file/stream or proprietary messaging packet. Binary output is usually smaller than XML or JSON.

# The XML Serializer

The XmlSerializer class in the `System.Xml.Serialization` namespace serializes and deserializes based on attributes in your classes.

## Getting Started with Attribute-Based Serialization

```csharp
public class Person
{
 public string Name;
 public int Age;
}
//The following saves a Person to an XML file and then restores it:
Person p = new Person();
p.Name = "Stacey"; p.Age = 30;
var xs = new XmlSerializer (typeof (Person));
using (Stream s = File.Create ("person.xml"))
 xs.Serialize (s, p);
Person p2;
using (Stream s = File.OpenRead ("person.xml"))
 p2 = (Person) xs.Deserialize (s);
Console.WriteLine (p2.Name + " " + p2.Age); // Stacey 30
```

Serialize and Deserialize can work with a Stream, XmlWriter/XmlReader, or TextWriter/TextReader. Here’s the resultant XML:

```xml
<?xml version="1.0"?>
<Person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:xsd="http://www.w3.org/2001/XMLSchema">
 <Name>Stacey</Name>
 <Age>30</Age>
</Person>
```

XmlSerializer can serialize types without any attributes—such as our Person type. By default, it serializes all public fields and properties on a type. You can exclude members that you don’t want serialized by applying the `XmlIgnore` attribute:

```csharp
public class Person
{
 ...
 [XmlIgnore] public DateTime DateOfBirth;
}
```

XmlSerializer relies on a parameterless constructor for deserialization, throwing an exception if one is not present. (In our example, Person has an implicit parame‐ terless constructor.) This also means that field initializers execute prior to deserialization:

```csharp
public class Person
{
 public bool Valid = true; // Executes before deserialization
}
```

Although XmlSerializer can serialize almost any type, it recognizes the following types and treats them specially:

- The primitive types, DateTime, TimeSpan, Guid, and nullable versions
- byte[] (which is converted to base 64)
- An XmlAttribute or XmlElement (whose contents are injected into the stream)
- Any type implementing IXmlSerializable
- Any collection type

### Attributes, names, and namespaces

By default, fields and properties serialize to an XML element. You can request an XML attribute be used, instead, as follows:

```csharp
[XmlAttribute] public int Age;
//You can control an element or attribute’s name as follows:
public class Person
{
 [XmlElement ("FirstName")] public string Name;
 [XmlAttribute ("RoughAge")] public int Age;
}

//Results
<Person RoughAge="30" ...>
 <FirstName>Stacey</FirstName>
</Person>
```

The default XML namespace is blank. To specify an XML namespace, [XmlElement] and [XmlAttribute] both accept a Namespace argument. You can also assign a name and namespace to the type itself with [XmlRoot]:

```csharp
[XmlRoot ("Candidate", Namespace = "http://mynamespace/test/")]
public class Person { ... }
```

### XML element order

XmlSerializer writes elements in the order in which they’re defined in the class. You can change this by specifying an Order in the XmlElement attribute:

```csharp
public class Person
{
 [XmlElement (Order = 2)] public string Name;
 [XmlElement (Order = 1)] public int Age;
}
```

If you use Order at all, you must use it throughout. The deserializer is not fussy about the order of elements—they can appear in any sequence and the type will properly deserialize.

## Subclasses and Child Objects

### Subclassing the root type

```csharp
//Suppose that your root type has two subclasses, as follows:
public class Person { public string Name; }
public class Student : Person { }
public class Teacher : Person { }
//and you want to write a reusable method to serialize the root type
public void SerializePerson (Person p, string path)
{
 XmlSerializer xs = new XmlSerializer (typeof (Person));
using (Stream s = File.Create (path))
 xs.Serialize (s, p);
}
```

To make this method work with a Student or Teacher, you must inform XmlSerializer about the subclasses. There are two ways to do this. The first is to register each subclass by applying the XmlInclude attribute:

```csharp
[XmlInclude (typeof (Student))]
[XmlInclude (typeof (Teacher))]
public class Person { public string Name; }
```

The second is to specify each of the subtypes when constructing XmlSerializer:

```csharp
XmlSerializer xs = new XmlSerializer (typeof (Person),
 new Type[] { typeof (Student), typeof (Teacher) } );
```

In either case, the serializer responds by recording the subtype in the type attribute:

```xml
<Person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:type="Student">
 <Name>Stacey</Name>
</Person>
```

This deserializer then knows from this attribute to instantiate a Student and not a Person.

You can control the name that appears in the XML type attribute by applying [XmlType] to the subclass:

```csharp
[XmlType ("Candidate")]
public class Student : Person { }

//Result
<Person xmlns:xsi="..."
 xsi:type="Candidate">
```

### Serializing child objects

XmlSerializer automatically recurses object references such as the HomeAddress field in Person:

```csharp
public class Person
{
 public string Name;
 public Address HomeAddress = new Address();
}
public class Address { public string Street, PostCode; }

//To demonstrate
Person p = new Person { Name = "Stacey" };
p.HomeAddress.Street = "Odo St";
p.HomeAddress.PostCode = "6020";

//XML
<Person ... >
 <Name>Stacey</Name>
 <HomeAddress>
 <Street>Odo St</Street>
 <PostCode>6020</PostCode>
 </HomeAddress>
</Person>
```

### Subclassing child objects

```csharp
public class Address { public string Street, PostCode; }
public class USAddress : Address { }
public class AUAddress : Address { }
public class Person
{
 public string Name;
 public Address HomeAddress = new USAddress();
}
```

There are two distinct ways to proceed, depending on how you want the XML structured. If you want the element name always to match the field or property name with the subtype recorded in a type attribute:

```csharp
<Person ...>
 ...
 <HomeAddress xsi:type="USAddress">
 ...
 </HomeAddress>
</Person>
```

you use [XmlInclude] to register each of the subclasses with Address, as follows:

```csharp
[XmlInclude (typeof (AUAddress))]
[XmlInclude (typeof (USAddress))]
public class Address
{
 public string Street, PostCode;
}
```

If, on the other hand, you want the element name to reflect the name of the subtype, to the following effect:

```csharp
<Person ...>
 ...
 <USAddress>
 ...
 </USAddress>
</Person>
```

you instead stack multiple [XmlElement] attributes onto the field or property in the parent type:

```csharp
public class Person
{
 public string Name;
 [XmlElement ("Address", typeof (Address))]
 [XmlElement ("AUAddress", typeof (AUAddress))]
 [XmlElement ("USAddress", typeof (USAddress))]
 public Address HomeAddress = new USAddress();
}
```

## Serializing Collections

XmlSerializer recognizes and serializes concrete collection types without intervention:

```csharp
public class Person
{
 public string Name;
 public List<Address> Addresses = new List<Address>();
}
public class Address { public string Street, PostCode; }
//Here’s the XML to which this serializes:
<Person ... >
 <Name>...</Name>
 <Addresses>
 <Address>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </Address>
 <Address>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </Address>
 ...
 </Addresses>
</Person>
```

- The [XmlArray] attribute lets you rename the outer element (i.e., Addresses).
- The [XmlArrayItem] attribute lets you rename the inner elements (i.e., the Address elements).

```csharp
public class Person
{
 public string Name;
 [XmlArray ("PreviousAddresses")]
 [XmlArrayItem ("Location")]
 public List<Address> Addresses = new List<Address>();
}
//serializes to this:
<Person ... >
 <Name>...</Name>
 <PreviousAddresses>
 <Location>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </Location>
 <Location>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </Location>
 ...
 </PreviousAddresses>
</Person>
```

- The XmlArray and XmlArrayItem attributes also allow you to specify XML namespaces.

### Working with subclassed collection elements

The rules for subclassing collection elements follow naturally from the other sub‐ classing rules. To encode subclassed elements with the type attribute, for example:

```csharp
<Person ... >
 <Name>...</Name>
 <Addresses>
 <Address xsi:type="AUAddress">
 ...
```

add [XmlInclude] attributes to the base (Address) type, as we did earlier. This works whether or not you suppress serialization of the outer element.

If you want subclassed elements to be named according to their type, for example:3

```xml
<Person ... >
 <Name>...</Name>
 <!—start of optional outer element—>
 <AUAddress>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </AUAddress>
 <USAddress>
 <Street>...</Street>
 <Postcode>...</Postcode>
 </USAddress>
 <!—end of optional outer element—>
</Person>
```

```csharp
//Stack multiple [XmlArrayItem] attributes if you want to include the outer collection
//element:
[XmlArrayItem ("Address", typeof (Address))]
[XmlArrayItem ("AUAddress", typeof (AUAddress))]
[XmlArrayItem ("USAddress", typeof (USAddress))]
public List<Address> Addresses = new List<Address>();

//Stack multiple [XmlElement] attributes if you want to exclude the outer collection
//element:
[XmlElement ("Address", typeof (Address))]
[XmlElement ("AUAddress", typeof (AUAddress))]
[XmlElement ("USAddress", typeof (USAddress))]
public List<Address> Addresses = new List<Address>();
```

### IXmlSerializable

Although attribute-based XML serialization is flexible, it has limitations. For instance, you cannot add serialization hooks—nor can you serialize nonpublic members. It’s also awkward to use if the XML might present the same element or attribute in a number of different ways.

On that last issue, you can push the boundaries somewhat by passing an XmlAttributeOverrides object into XmlSerializer’s constructor. There comes a point, however, when it’s easier to take an imperative approach. This is the job of IXmlSerializable:

```csharp
public interface IXmlSerializable
{
 XmlSchema GetSchema();
 void ReadXml (XmlReader reader);
 void WriteXml (XmlWriter writer);
}
```

```csharp
//Here’s an example:
using System;
using System.Xml;
using System.Xml.Schema;
using System.Xml.Serialization;
public class Address : IXmlSerializable
{
 public string Street, PostCode;
 public XmlSchema GetSchema() { return null; }
 public void ReadXml(XmlReader reader)
 {
 reader.ReadStartElement();
 Street = reader.ReadElementContentAsString ("Street", "");
 PostCode = reader.ReadElementContentAsString ("PostCode", "");
 reader.ReadEndElement();
 }
 public void WriteXml (XmlWriter writer)
 {
 writer.WriteElementString ("Street", Street);
 writer.WriteElementString ("PostCode", PostCode);
}
}
```

Serializing and deserializing an instance of Address via XmlSerializer automati‐ cally calls the WriteXml and ReadXml methods. Further, if Person were defined like this:

```csharp
public class Person
{
 public string Name;
 public Address HomeAddress;
}
```

IXmlSerializable would be called upon selectively to serialize the HomeAddress field.

# The JSON Serializer

JsonSerializer (in the System.Text.Json namespace) is straightforward to use because of the simplicity of the JSON format. The root of a JSON document is either an array or an object. Under that root are properties, which can be an object, array, string, number, "true", "false", or "null". The JSON serializer directly maps class property names to property names in JSON.

## Getting Started

```csharp
public class Person
{
 public string Name { get; set; }
}
var p = new Person { Name = "Ian" };
string json = JsonSerializer.Serialize (p,
 new JsonSerializerOptions { WriteIndented = true });

//result:
{
 Name: "Ian"
}

//JsonSerializer.Deserialize method does the reverse
Person p2 = JsonSerializer.Deserialize<Person> (json);
```

## Serializing Child Objects

```csharp
public class Address
{
 public string Street { get; set; }
 public string PostCode { get; set; }
}
public class Person
{
 public string Name { get; set; }
 public Address HomeAddress { get; set; }
 public Address WorkAddress { get; set; }
}
//We can serialize this with no extra work
var home = new Address { Street = "1 Main St.", PostCode="11235" };
var work = new Address { Street = "4 Elm Ln.", PostCode="31415" };
var p = new Person { Name = "Ian", HomeAddress = home, WorkAddress = work };
Console.WriteLine (JsonSerializer.Serialize (p,
 new JsonSerializerOptions { WriteIndented = true } ));
```

```json
{
 "Name": "Ian",
 "HomeAddress": {
 "Street": "1 Main St.",
 "PostCode": "11235"
 },
 "WorkAddress": {
 "Street": "4 Elm Ln.",
 "PostCode": "31415"
 }
}
```

## Serializing Collections

JsonSerializer automatically serializes collections. Collections can appear in an object’s properties as well as in the root object itself

```csharp
var sara = new Person { Name = "Sara" };
var ian = new Person { Name = "Ian" };
Console.WriteLine (JsonSerializer.Serialize (new[] { sara, ian },
 new JsonSerializerOptions { WriteIndented = true }));
//Here’s the result:
[
 {
 "Name": "Sara"
 },
 {
 "Name": "Ian"
 }
]
//The following deserializes the JSON:
Person[] people = JsonSerializer.Deserialize<Person[]> (json);
```

## Controlling Serialization with Attributes

You can control the serialization process with attributes defined in the `System.Text.Json.Serialization namespace`.

- JsonIgnoreAttribute
    
    To Opt Out of Specific Property
    
- JsonPropertyNameAttribute
    
    To give Specific Name to Property
    
- JsonExtensionDataAttribute
    
    providing a mechanism to
    store all unrecognized properties so that their values can be used when reserializing
    
- JsonConverterAttribute
    
    ```csharp
    {
     "Id":27182,
     "Name":"Sara",
     "Born":464572800 // Number of seconds since 1/1/1970
    }
    
    public class Person
    {
     public int Id { get; set; }
     public string Name { get; set; }
     public DateTime Born { get; set; }
    }
    public class UnixTimestampConverter : JsonConverter<DateTime>
    {
     public override DateTime Read (ref Utf8JsonReader reader, Type type,
     JsonSerializerOptions options)
     {
     if (reader.TryGetInt32(out int timestamp))
     return new DateTime (1970, 1, 1).AddSeconds (timestamp);
     throw new Exception ("Expected the timestamp as a number.");
     }
     public override void Write (Utf8JsonWriter writer, DateTime value,
     JsonSerializerOptions options)
     {
     int timestamp = (int)(value - new DateTime(1970, 1, 1)).TotalSeconds;
     writer.WriteNumberValue(timestamp);
     }
    }
    //Then we can either apply the [JsonConverter] to the properties that we want to
    //convert:
    [JsonConverter(typeof(UnixTimestampConverter))]
    public DateTime Born { get; set; }
    //or, if the API is consistent in its representation of data types, make the 
    //converter act as a default:
    JsonSerializerOptions opts = new JsonSerializerOptions();
    opts.Converters.Add (new UnixTimestampConverter());
    var sara = JsonSerializer.Deserialize<Person> (json, opts);
    ```
    

## JSON Serialization Options

The serializer accepts an optional JsonSerializationOptions parameter, allowing additional control over the serialization and deserialization process.

### WriteIndented

We have set WriteIndented to true throughout this section to instruct the serializer to emit whitespace to generate more human-readable JSON. The default is false, which results in everything being crammed onto one line.

### AllowTrailingCommas

The JSON spec requires properties and array elements to be comma separated but does not allow trailing commas:

```csharp
{
 "Name":"Dylan",
 "LuckyNumbers": [10, 7, ],
 "Age":46,
}
//The trailing commas after 7 and 46 are not allowed by default.
```

### ReadCommentHandling

By default, the deserializer throws an exception when encountering comments (because comments are not part of the official JSON standard). Setting ReadComment Handling to JsonCommentHandling.Skip instructs the deserializer to skip over them instead, so the following can be successfully parsed:

```csharp
{
 "Name":"Dylan" // Comment here
 /* This is another comment */
}
```

### PropertyNameCaseInsensitive

By default, the deserializer is case sensitive when matching JSON property names to C# property names. This means that the following input:
`{ "name":"Dylan" }`
would fail to populate the Name property in our Person class (the JSON property would be ignored).

### PropertyNamingPolicy

To better support the popular camel-case property naming convention, .NET Core 3 introduced PropertyNamingPolicy. It provides better performance than the justdescribed PropertyNameCaseInsensitive option and applies to both serialization and deserialization.

```csharp
var dylan = new Person { Name = "Dylan" };
var json = JsonSerializer.Serialize (dylan,
 new JsonSerializerOptions{
 PropertyNamingPolicy = JsonNamingPolicy.CamelCase
 });
yields:
{"name": "Dylan"}
which can be deserialized in the same way:
var dylan2 = JsonSerializer.Deserialize<Person> (json,
 new JsonSerializerOptions
 {
 PropertyNamingPolicy = JsonNamingPolicy.CamelCase
 });
```

### DictionaryKeyPolicy

With the DictionaryKeyPolicy option, you can force dictionary keys to serialize or deserialize with camel casing:

```csharp
var dict = new Dictionary<string, string>
{
 { "BookName", "Nutshell" }
 { "BookVersion", "8.0" },
};
Console.WriteLine (JsonSerializer.Serialize (dict,
 new JsonSerializerOptions
 {
 WriteIndented = true,
 DictionaryKeyPolicy = JsonNamingPolicy.CamelCase
 }));
This outputs the following:
{
 "bookName": "Nutshell"
 "bookVersion": "8.0",
}
```

### Encoder

The default text encoder aggressively escapes characters such that the output can appear in an HTML document without additional processing:

```csharp
string dylan = "<b>Dylan & Friends</b>";
Console.WriteLine (JsonSerializer.Serialize (dylan));
Here’s the output:
"\u003Cb\u003EDylan \u0026 Friends\u003C/b\u003E"
//You can prevent this by changing the Encoder:
Console.WriteLine (JsonSerializer.Serialize (dylan,
 new JsonSerializerOptions {Encoder = JavaScriptEncoder.UnsafeRelaxedJsonEscaping
 }));
//This yields the following output:
"<b>Dylan & Friends</b>"
```

### IgnoreNullValues

```csharp
var person = new Person { Name = null };
//would serialize to:
{
 "Name": null
}
With IgnoreNullValues set to true, null-value properties are completely ignored:
Console.WriteLine (JsonSerializer.Serialize (person),
 new JsonSerializerOptions { IgnoreNullValues = true } ));
//Here’s the output:
{}
```

### IgnoreReadOnlyProperties

By default, read-only properties are serialized (but not deserialized, because there is no set accessor). You can tell the serializer to ignore read-only properties by setting IgnoreReadOnlyProperties to true

# The Binary Serializer

The binary serialization engine saves and restores objects with full type and reference fidelity, and you can use it to perform such tasks as saving and restoring objects to disk. The binary serializer is highly automated and can handle complex object graphs with minimum intervention.

There are two ways to make a type support binary serialization. The first is attribute-based; the second involves implementing ISerializable. Adding attributes is simpler; implementing ISerializable is more flexible. You typically implement ISerializable to do the following

- Dynamically control what gets serialized
- Make your serializable type friendly to being subclassed by other parties

## Getting Started

You can make a type serializable by applying a single attribute

```csharp
[Serializable] public sealed class Person
{
 public string Name;
 public int Age;
}
```

The [Serializable] attribute instructs the serializer to include all fields in the type. This includes both private and public fields (but not properties). Every field must itself be serializable; otherwise, an exception is thrown. Primitive .NET types such as string and int support serialization (as do many other .NET types).

<aside>
👉🏿 The Serializable attribute is not inherited, so subclasses are not automatically serializable, unless also marked with this attribute.

</aside>

To serialize an instance of Person, you instantiate BinaryFormatter (in System .Runtime.Serialization.Formatters.Binary) and call Serialize.

```csharp
Person p = new Person() { Name = "George", Age = 25 };
IFormatter formatter = new BinaryFormatter();
using (FileStream s = File.Create ("serialized.bin"))
 formatter.Serialize (s, p);
```

All of the data necessary to reconstruct the Person object is written to the file serialized.bin. The Deserialize method restores the object:

```csharp
using (FileStream s = File.OpenRead ("serialized.bin"))
{
 Person p2 = (Person) formatter.Deserialize (s);
 Console.WriteLine (p2.Name + " " + p2.Age); // George 25
}
```

<aside>
👉🏿 The deserializer bypasses all constructors and field initializers when re-creating objects. Behind the scenes, it calls Formatter Services.GetUninitializedObject to do this job. You can call this method yourself to implement some very grubby design patterns!

</aside>

The serialized data includes full type and assembly information, so if we try to cast the result of deserialization to a matching Person type in a different assembly, an error would result. The deserializer fully restores object references to their original state upon deserialization. This includes collections, which are just treated as serializable objects like any other

The binary engine can handle large, complex object graphs without special assistance (other than ensuring that all partici‐ pating members are serializable). One thing to be wary of is that the serializer’s performance degrades in proportion to the number of references in your object graph. This can become an issue in a Remoting server that has to process many con‐ current requests.

# Binary Serialization Attributes

## [NonSerialized]

By default, all fields are serialized. Fields that you don’t want serialized, such as those used for temporary calculations or for storing file or window handles, you must mark explicitly with the [NonSerialized] attribute:

## [OnDeserializing]

A method marked with the [OnDeserializing] attribute fires just prior to deserial‐ ization and acts as a kind of constructor. This can be important because the binary deserializer bypasses all your normal constructors as well as field initializers.

In the following example, we define a field called Valid, which we exclude from serialization with the [NonSerialized] attribute:

```csharp
public sealed class Person
{
 public string Name;
 [NonSerialized] public bool Valid = true;
 public Person() => Valid = true;
}
```

A deserialized Person will never be Valid—despite the constructor and field initial‐ izer both setting Valid to true. We can solve this by writing a special deserialization constructor as follows:

```csharp
[OnDeserializing]
void OnDeserializing (StreamingContext context) => Valid = true;
```

## [OnDeserialized]

A method marked with the [OnDeserialized] attribute fires just after deserializa‐ tion. This can be useful for updating calculated fields, and in conjunction with [OnSerializing], which we look at next.

## [OnSerializing] and [OnSerialized]

The [OnSerializing] and [OnSerialized] attributes mark methods for execution before or after serialization.[OnSerializing] is useful for populating a field that’s used only for serialization.

```csharp
class Foo
{
 public XDocument Xml;
}
```

The difficulty is that XDocument (in the System.Xml.Linq namespace) is not itself serializable. We can solve this by applying the [NonSerialized] attribute to the Xml field and then defining an [OnSerializing] method that writes the content of the XDocument to a string field (that we do serialize):

```csharp
[Serializable]
class Foo
{
 [NonSerialized]
 public XDocument Xml;
 string _xmlString; // used only for serialization
 [OnSerializing]
 void OnSerializing (StreamingContext context)
 => _xmlString = Xml.ToString();
}
```

The final step is to reconstruct the XDocument when deserializating. We can do this by adding an [OnDeserialized] method:

```csharp
[OnDeserialized]
 void OnDeserialized (StreamingContext context)
 => Xml = XDocument.Parse (_xmlString);
```

## [OptionalField] and Versioning

Adding or removing fields doesn’t break compatibility with already serialized data: the deserializer skips over data for which there’s no matching field. When adding a field, you can apply the following attribute to remind yourself that it might be absent from data serialized by an older version of the software:

```csharp
[Serializable] public sealed class Person
{
 public string Name;
 [OptionalField (VersionAdded = 2)] public DateTime DateOfBirth;
}
```

# Binary Serialization with ISerializable

Implementing ISerializable gives a type complete control over its binary serialization and deserialization.

```csharp
//Here’s the ISerializable interface definition:
public interface ISerializable
{
 void GetObjectData (SerializationInfo info, StreamingContext context);
}
```

GetObjectData fires upon serialization; its job is to populate the Serialization Info object (a name-value dictionary) with data from all fields that you want serial‐ ized. Here’s how we would write a GetObjectData method that serializes two fields, called Name and DateOfBirth:

```csharp
public virtual void GetObjectData (SerializationInfo info,
 StreamingContext context)
 {
 info.AddValue ("Name", Name);
 info.AddValue ("DateOfBirth", DateOfBirth);
 }
```

In the following example, we define Player and Team classes, following the principles of immutability (with everything read-only). But because the immutable collections are not serializable, we need to take control over the serialization process by implementing ISerializable:

```csharp
[Serializable] public class Player
{
 public readonly string Name;
 public Player (string name) => Name = name;
}
[Serializable] public class Team : ISerializable
{
 public readonly string Name;
 public readonly ImmutableList<Player> Players; // Not serializable!
 public Team (string name, params Player[] players)
 {
 Name = name;
 Players = players.ToImmutableList();
 }
 // Serialize the object:
 public virtual void GetObjectData (SerializationInfo si,
 StreamingContext sc)
 {
 si.AddValue ("Name", Name);
 // Convert Players to an ordinary serializable array:
 si.AddValue ("PlayerData", Players.ToArray());
 }
 // Deserialize the object:
 protected Team (SerializationInfo si, StreamingContext sc)
 {
 Name = si.GetString ("Name");
 // Deserialize Players to an array to match our serialization:
 Player[] p = (Player[]) si.GetValue ("PlayerData", typeof (Player[]));
 // Construct a new immutable List using this array:
Players = p.ToImmutableList();
 }
}
```

## Subclassing Serializable Classes

```csharp
[Serializable] public class Person
{
 public string Name;
 public int Age;
}
[Serializable] public sealed class Student : Person
{
 public string Course;
}
```

In this example, both Person and Student are serializable, and both classes use the default runtime serialization behavior because neither class implements ISerializable. Now imagine that the developer of Person decides for some reason to implement ISerializable and provide a deserialization constructor to control Person seriali‐ zation. The new version of Person might look like this:

```csharp
[Serializable] public class Person : ISerializable
{
 public string Name;
 public int Age;
 public virtual void GetObjectData (SerializationInfo si,
 StreamingContext sc)
 {
 si.AddValue ("Name", Name);
 si.AddValue ("Age", Age);
 }
 protected Person (SerializationInfo si, StreamingContext sc)
 {
 Name = si.GetString ("Name");
 Age = si.GetInt32 ("Age");
 }
 public Person() {}
}
```

Although this works for instances of Person, this change breaks serialization of Student instances. Serializing a Student instance would appear to succeed, but the Course field in the Student type isn’t saved to the stream because the implementation of ISerializable.GetObjectData on Person has no knowledge of the members of the Student-derived type. Additionally, deserialization of Student instances throws an exception because the runtime is looking (unsuccessfully) for a deserialization constructor on Student.

The solution to this problem is to implement ISerializable from the outset for serializable classes that are public and nonsealed. (With internal classes, it’s not so important because you can easily modify the subclasses later if required.)

```csharp
[Serializable]
public class Student : Person
{
 public string Course;
 public override void GetObjectData (SerializationInfo si,
 StreamingContext sc)
 {
 base.GetObjectData (si, sc);
si.AddValue ("Course", Course);
 }
 protected Student (SerializationInfo si, StreamingContext sc)
 : base (si, sc)
 {
 Course = si.GetString ("Course");
 }
 public Student() {}
}
```# Chapter 18 : Assemblies

An assembly is the basic unit of deployment in .NET Core and is also the container for all types. An assembly contains compiled types with their IL code, runtime resources, and information to assist with versioning and referencing other assemblies. An assembly also defines a boundary for type resolution. In .NET Core, an assembly comprises a single file with a .dll extension.

When you build an executable application in .NET Core, you end up with two files: an assembly (.dll) and an executable launcher (.exe) appropriate to the platform you’re targeting. 

This differs to what happens in .NET Framework, which generates a portable executable (PE) assembly. A PE has an .exe extension and acts both as an assembly and an application launcher. A PE can simultaneously target 32- and 64-bit versions of Windows.

Most of the types in this chapter come from the following namespaces

```csharp
System.Reflection
System.Resources
System.Globalization
```

# What’s in an Assembly

- An assembly manifest
    
    Provides information to the CLR, such as the assembly’s name, version, and other assemblies that it references
    
- An application manifest
    
    Provides information to the operating system, such as how the assembly should be deployed and whether administrative elevation is required
    
- Compiled types
    
    The compiled IL code and metadata of the types defined within the assembly
    
- Resources
    
    Other data embedded within the assembly, such as images and localizable text
    

## The Assembly Manifest

The assembly manifest serves two purposes

- It describes the assembly to the managed hosting environment.
- It acts as a directory to the modules, types, and resources in the assembly.

Assemblies are thus self-describing. A consumer can discover all of an assembly’s data, types, and functions—without needing additional files.

<aside>
📢 An assembly manifest is not something you add explicitly to an assembly—it’s automatically embedded into an assembly as part of compilation.

</aside>

Here’s a summary of the functionally significant data stored in the manifest

- The simple name of the assembly
- A version number (AssemblyVersion)
- A public key and signed hash of the assembly, if strongly named
- A list of referenced assemblies, including their version and public key
- A list of types defined in the assembly
- The culture it targets, if a satellite assembly (AssemblyCulture)

The manifest can also store the following informational data

- A full title and description (AssemblyTitle and AssemblyDescription)
- Company and copyright information (AssemblyCompany and Assembly Copyright)
- A display version (AssemblyInformationalVersion)
- Additional attributes for custom data

### Specifying assembly attributes

Commonly used assembly attributes can be specified in Visual Studio on the project’s Properties page, on the Package tab. The settings on that tab are added to the project file (.csproj).

To specify attributes not supported by the Package tab, or if not working with a .csproj file, you can specify assembly attributes in source code. .NET Framework projects automatically create a file for this purpose, AssemblyInfo.cs in the Properties folder, but .NET Core projects do not. Although you can specify attributes in any source code file in your project, adding a .cs file specifically for attributes allows you to keep them together and well organized.

## The Application Manifest (Windows)

An application manifest is an XML file that communicates information about the assembly to the OS. An application manifest is embedded into the startup executable as a Win32 resource during the build process. If present, the manifest is read and processed before the CLR loads the assembly—and can influence how Windows launches the application’s process.

A .NET application manifest has a root element called assembly in the XML name‐ space urn:schemas-microsoft-com:asm.v1

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
 <!-- contents of manifest -->
</assembly>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
 <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">
 <security>
 <requestedPrivileges>
 <requestedExecutionLevel level="requireAdministrator" />
</requestedPrivileges>
 </security>
 </trustInfo>
</assembly>
```

### Deploying an application manifest

You can add an application manifest to a .NET Core project in Visual Studio by right-clicking your project in Solution Explorer, selecting Add, then “New item,” and then choosing Application Manifest File. Upon building, the manifest will be embedded into the output assembly.

## Modules

The contents of an assembly are actually packaged within an intermediate container, called a module. A module corresponds to a file containing the contents of an assembly. The reason for this extra layer of containership is to allow an assembly to span multiple files, a feature present in .NET Framework but absent in .NET Core.

## The Assembly Class

The Assembly class in `System.Reflection` is a gateway to accessing assembly meta‐ data at runtime. There are a number of ways to obtain an assembly object: the simplest is via a Type’s Assembly property:

```csharp
Assembly a = typeof (Program).Assembly;
```

- GetExecutingAssembly
    
    Returns the assembly of the type that defines the currently executing function
    
- GetCallingAssembly
    
    Does the same as GetExecutingAssembly but for the function that called the currently executing function
    
- GetEntryAssembly
    
    Returns the assembly defining the application’s original entry method
    

# Strong Names and Assembly Signing

A strongly named assembly has a unique identity. It works by adding two bits of metadata to the manifest:

- A unique number that belongs to the authors of the assembly
- A signed hash of the assembly, proving that the unique number holder produced the assembly

This requires a public/private key pair. The public key provides the unique identify‐ ing number, and the private key facilitates signing.

The public key is valuable in guaranteeing the uniqueness of assembly references: a strongly named assembly incorporates the public key into its identity.

Strongly naming an assembly is important in .NET Frame‐ work for two reasons

- It allows the assembly to be loaded into the “global assembly cache”
- It allows the assembly to by referenced by other strongly named assemblies.

In .NET Framework, the private key protects your assembly from tampering, in that without your private key, no one can release a modified version of the assembly without the signature breaking. In practice, this is of use when loading an assembly into .NET Framework’s global assembly cache.

## How to Strongly Name an Assembly

To give an assembly a strong name, first generate a public/private key pair with the sn.exe utility

```csharp
sn.exe -k MyKeyPair.snk
```

<aside>
📢 Visual Studio installs a shortcut called Developer Command Prompt for VS, which starts a command prompt whose PATH contains development tools such as sn.exe.

</aside>

This manufactures a new key pair and stores it to a file called MyKeyPair.snk. If you subsequently lose this file, you will permanently lose the ability to recompile your assembly with the same identity.

You can sign an assembly with this file by updating your project file. From Visual Studio, go to the Project Properties window, and then, on the Signing tab, select the “Sign the assembly” checkbox and select your .snk file.

The same key pair can sign multiple assemblies—they’ll still have distinct identities if their simple names differ.

# Assembly Names

An assembly’s “identity” comprises four pieces of metadata from its manifest

- Its simple name
- Its version (“0.0.0.0” if not present)
- Its culture (“neutral” if not a satellite)
- Its public key token (“null” if not strongly named)

The simple name comes not from any attribute, but from the name of the file to which it was originally compiled (less any extension). So, the simple name of the System.Xml.dll assembly is “System.Xml.” Renaming a file doesn’t change the assembly’s simple name.

The version number comes from the AssemblyVersion attribute. It’s a string divided into four parts as follows

```csharp
major.minor.build.revision
```

You can specify a version number as follows:

```csharp
[assembly: AssemblyVersion ("2.5.6.7")]
```

## Fully Qualified Names

A fully qualified assembly name is a string that includes all four identifying components, in this format

```csharp
simple-name, Version=version, Culture=culture, PublicKeyToken=public-key
```

or example, the fully qualified name of System.Private.CoreLib.dll is System.Pri‐ vate.CoreLib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e.

### The AssemblyName Class

AssemblyName is a class with a typed property for each of the four components of a fully qualified assembly name. AssemblyName has two purposes:

- It parses or builds a fully qualified assembly name
- It stores some extra data to assist in resolving (finding) the assembly.

You can obtain an AssemblyName object in any of the following ways

- Instantiate an AssemblyName, providing a fully qualified name
- Call GetName on an existing Assembly
- Call AssemblyName.GetAssemblyName, providing the path to an assembly file on disk

You can also instantiate an AssemblyName object without any arguments and then set each of its properties to build a fully qualified name. An AssemblyName is mutable when constructed in this manner

```csharp
//To use AssemblyName to obtain the simple name of an assembly:
Console.WriteLine (typeof (string).Assembly.GetName().Name);
// System.Private.CoreLib
//To get an assembly version:
string v = myAssembly.GetName().Version.ToString();
```

## Assembly Informational and File Versions

Two further assembly attributes are available for expressing version-related infor‐ mation. Unlike AssemblyVersion, the following two attributes do not affect an assembly’s identity and so have no effect on what happens at compile-time or at runtime:

- AssemblyInformationalVersion
    
    The version as displayed to the end user. This is visible in the Windows File Properties dialog box as Product Version. Any string can go here, such as “5.1 Beta 2.” Typically, all of the assemblies in an application would be assigned the same informational version number.
    
- AssemblyFileVersion
    
    This is intended to refer to the build number for that assembly. This is visible in the Windows File Properties dialog box as File Version. As with Assembly Version, it must contain a string consisting of up to four numbers separated by periods.
    

# Authenticode Signing

Authenticode is a code-signing system whose purpose is to **prove the identity of the publisher**. Authenticode and strong-name signing are independent: you can sign an assembly with either or both systems.

Although strong-name signing can prove that assemblies A, B, and C came from the same party (assuming the private key hasn’t been leaked), it can’t tell you who that party was. To know that the party was Joe Albahari—or Microsoft Corporation— you need Authenticode.

Authenticode is useful when downloading programs from the internet, because it provides assurance that a program came from whoever was named by the Certificate Authority and was not modified in transit. It also prevents the Unknown Publisher warning when running a downloaded application for the first time.

Authenticode signing is also a requirement when submitting apps to the Windows Store.

Authenticode works with not only .NET assemblies, but also unmanaged executables and binaries such as .msi deployment files. Of course, Authenticode doesn’t guarantee that a program is free from malware—although it does make it less likely. A person or entity has been willing to put its name (backed by a passport or company document) behind the executable or library.

Signing with Authenticode requires that you contact a Certificate Authority (CA) with evidence of your personal identity or company’s identity (articles of incorporation, etc.). After the CA has checked your documents, it will issue an X.509 codesigning certificate that is typically valid for one to five years. This enables you to sign assemblies with the signtool utility. You can also make a certificate yourself with the makecert utility; however, it will be recognized only on computers on which the certificate is explicitly installed.

The fact that (non-self-signed) certificates can work on any computer relies on public key infrastructure. Essentially, your certificate is signed with another certificate belonging to a CA. The CA is trusted because all CAs are loaded into the OS (to see them, go to the Windows Control Panel and then, in the search box, type “certificate”. In the Administrative Tools section, click “Manage computer certificates.” This launches the Certificate Manager. Open the node Trusted Root Certification Authorities and click Certificates). A CA can revoke a publisher’s certificate if leaked, so verifying an Authenticode signature requires periodically asking the CA for an up-to-date list of certification revocations. **Because Authenticode uses cryptographic signing, an Authenticode signature is invalid if someone subsequently tampers with the file.**

## Verifying that a program has been signed

The easiest way to view an Authenticode signature on a file is to view the file’s properties in Windows Explorer (look in the Digital Signatures tab). The signtool utility also provides an option for this.

# Resources and Satellite Assemblies

An application typically contains not only executable code, but also content such as text, images, or XML files. Such content can be represented in an assembly through a resource. There are two overlapping use cases for resources:

- Incorporating data that cannot go into source code, such as images
- Storing data that might need translation in a multilingual application

## Directly Embedding Resources

To directly embed a resource using Visual Studio:

- Add the file to your project
- Set its build action to Embedded Resource

Visual Studio always prefixes resource names with the project’s default namespace, plus the names of any subfolders in which the file is contained. So, if your project’s default namespace was `Westwind.Reports` and your file was called banner.jpg in the folder pictures, the resource name would be `Westwind.Reports.pictures.banner.jpg`. **Resource names are case sensitive**. This makes project sub‐ folder names in Visual Studio that contain resources effec‐ tively case sensitive.

To retrieve a resource, you call GetManifestResourceStream on the assembly con‐ taining the resource. This returns a stream, which you can then read as any other

```csharp
Assembly a = Assembly.GetEntryAssembly();
using (Stream s = a.GetManifestResourceStream ("TestProject.data.xml"))
using (XmlReader r = XmlReader.Create (s))
 ...
System.Drawing.Image image;
using (Stream s = a.GetManifestResourceStream ("TestProject.banner.jpg"))
 image = System.Drawing.Image.FromStream (s);
```

## .resources Files

.resources files are containers for potentially localizable content. A .resources file ends up as an embedded resource within an assembly—just like any other kind of file. The difference is that you must do the following:

- Package your content into the .resources file to begin with
- Access its content through a ResourceManager or pack URI rather than a Get ManifestResourceStream

.resources files are structured in binary and so are not human-editable; therefore, you must rely on tools provided by the Framework and Visual Studio to work with them. The standard approach with strings or simple data types is to use the .resx format, which can be converted to a .resources file either by Visual Studio or the resgen tool. The .resx format is also suitable for images intended for a Windows Forms or [ASP.NET](http://asp.net/) application.

## Satellite Assemblies

Data embedded in .resources is localizable. Resource localization is relevant when your application runs on a version of Windows built to display everything in a different language. For consistency, your application should use that same language, too.

A typical setup is as follows

- The main assembly contains .resources for the default or fallback language.
- Separate satellite assemblies contain localized .resources translated to different languages

When your application runs, .NET Core examines the language of the current OS (from `CultureInfo.CurrentUICulture`). Whenever you request a resource using `ResourceManager`, the Framework looks for a localized satellite assembly. If one’s available—and it contains the resource key you requested—it’s used in place of the main assembly’s version.

This means that you can enhance language support simply by adding new satellites without changing the main assembly. A satellite assembly cannot contain executable code, only resources.

Satellite assemblies are deployed in subdirectories of the assembly’s folder as follows:

```csharp
programBaseFolder\MyProgram.exe
 \MyLibrary.exe
 \XX\MyProgram.resources.dll
 \XX\MyLibrary.resources.dll
```

**XX refers to the two-letter language code (such as “de” for German) or a language and region code (such as “en-GB” for English in Great Britain). This naming system allows the CLR to find and load the correct satellite assembly automatically.**

### Building satellite assemblies

```csharp
//Recall our previous .resx example, which included the following:
<root>
 ...
 <data name="Greeting"
 <value>hello</value>
 </data>
</root>
```

We then retrieved the greeting at runtime as follows

```csharp
ResourceManager r = new ResourceManager ("welcome",
 Assembly.GetExecutingAssembly());
Console.Write (r.GetString ("Greeting"));
```

Suppose that we want this to instead write “hallo” if running on the German version of Windows. The first step is to add another .resx file named welcome.de.resx that substitutes hello for hallo:

```csharp
<root>
 <data name="Greeting">
 <value>hallo<value>
 </data>
</root>
```

In Visual Studio, this is all you need to do—when you rebuild, a satellite assembly called MyApp.resources.dll is automatically created in a subdirectory called de.

## Cultures and Subcultures

Cultures are split into cultures and subcultures. A culture represents a particular language; a subculture represents a regional variation of that language. The Frame‐ work follows the RFC1766 standard, which represents cultures and subcultures with two-letter codes. Here are the codes for English and German cultures:

```csharp
En
de
```

Here are the codes for the Australian English and Austrian German subcultures:

```csharp
en-AU
de-AT
```

A culture is represented in .NET with the System.Globalization.CultureInfo class. You can examine the current culture of your application as follows:

```csharp
Console.WriteLine (System.Threading.Thread.CurrentThread.CurrentCulture);
Console.WriteLine (System.Threading.Thread.CurrentThread.CurrentUICulture);
```

`CurrentCulture` reflects the regional settings of the Windows Control Panel, whereas `CurrentUICulture` reflects the language of the OS.# Chapter 2 : C# Language Basics

### First C# Program

```csharp
using System;
class Test
{
 static void Main()
 {
 Console.WriteLine (FeetToInches (30)); // 360
 Console.WriteLine (FeetToInches (100)); // 1200
 }
 static int FeetToInches (int feet)
 {
 int inches = feet * 12;
 return inches;
 }
}
```

At the outermost level of a program, types are organized into namespaces. The
using directive makes the System namespace available to our application, to use the
Console class. We could define all of our classes within the TestPrograms name‐
space, as follows:

```csharp
using System;
namespace TestPrograms
{
 class Test {...}
 class Test2 {...}
}
```

The .NET Core libraries are organized into nested namespaces. For example, this is
the namespace that contains types for handling text: `using System.Text`

### Identifiers and Keywords

Identifiers are names that programmers choose for their classes, methods, variables,
and so on. Here are the identifiers in our example program, in the order in which
they appear:

```csharp
System Test Main x Console WriteLine
```

<aside>
💡 An identifier must be a whole word, essentially made up of Unicode characters
starting with a letter or underscore. C# identifiers are case sensitive. By convention,
parameters, local variables, and private fields should be in camel case (e.g., my
Variable), and all other identifiers should be in Pascal case (e.g., MyMethod).

</aside>

Keywords are names that mean something special to the compiler. These are the
keywords in our example program:

```csharp
using class static void int
```

Most keywords are reserved, which means that you can’t use them as identifiers.

- list of C# reserved keywords
    
    ```csharp
    abstract
    as
    base
    bool
    break
    byte
    case
    catch
    char
    checked
    class
    const
    continue
    decimal
    default
    delegate
    do
    double
    else
    enum
    event
    explicit
    extern
    false
    finally
    fixed
    float
    for
    foreach
    goto
    if
    implicit
    in
    int
    interface
    internal
    is
    lock
    long
    namespace
    new
    null
    object
    operator
    out
    override
    params
    private
    protected
    public
    readonly
    ref
    return
    sbyte
    sealed
    short
    sizeof
    stackalloc
    static
    string
    struct
    switch
    this
    throw
    true
    try
    typeof
    uint
    ulong
    unchecked
    unsafe
    ushort
    using
    virtual
    void
    volatile
    while
    ```
    

### Avoiding conflicts

If you really want to use an identifier that clashes with a reserved keyword, you can
do so by qualifying it with the @ prefix.

```csharp
class class {...} // Illegal
class @class {...} // Legal
```

### Contextual keywords

Some keywords are contextual, meaning that you also can use them as identifiers without an @ symbol:

- Code
    
    ```csharp
    add
    alias
    ascending
    async
    await
    by
    descending
    dynamic
    equals
    from
    get
    global
    group
    in
    into
    join
    let
    nameof
    on
    orderby
    partial
    remove
    select
    set
    unmanaged
    value
    var
    when
    where
    yield
    ```
    
    - 

### Instance versus static members

The data members and function members that operate on the instance of the type are called instance members. The UnitConverter’s Convert method and the int’s ToString method are examples of instance members. By default, members are instance members.

**Data members and function members that don’t operate on the instance of the type but rather on the type itself must be marked as static.** The Test.Main and Console.WriteLine methods are static methods. **The Console class is actually a static class, which means that all of its members are static.** You never actually create instances of a Console—one console is shared across the entire application.

```csharp
public class Panda
{
 public string Name; // Instance field
 public static int Population; // Static field
 public Panda (string n) // Constructor
 {
 Name = n; // Assign the instance field
 Population = Population + 1; // Increment the static Population field
 }
}

using System;
class Test
{
 static void Main()
 {
 Panda p1 = new Panda ("Pan Dee");
 Panda p2 = new Panda ("Pan Dah");
 Console.WriteLine (p1.Name); // Pan Dee
 Console.WriteLine (p2.Name); // Pan Dah
 Console.WriteLine (Panda.Population); // 2
 }
}
```

### Conversions

C# can convert between instances of compatible types. A conversion always creates
a new value from an existing one. Conversions can be either implicit or explicit:
implicit conversions happen automatically, and explicit conversions require a cast.

```csharp
int x = 12345; // int is a 32-bit integer
long y = x; // Implicit conversion to 64-bit integer
short z = (short)x; // Explicit conversion to 16-bit integer
```

Implicit conversions are allowed when both of the following are true

- The compiler can guarantee that they will always succeed
- **No information is lost in conversion**

Explicit conversions are required when one of the following is true

- The compiler cannot guarantee that they will always succeed
- **Information might be lost during conversion**

### Overflow check operators

The checked operator instructs the runtime to generate an OverflowException
rather than overflowing silently when an integral-type expression or statement
exceeds the arithmetic limits of that type.

You can use checked around either an expression or a statement block

```csharp
int a = 1000000;
int b = 1000000;
int c = checked (a * b); // Checks just the expression.
checked // Checks all expressions
{ // in statement block.
 ...
 c = a * b;
 ...
}
```

You can make arithmetic overflow checking the default for all expressions in a pro‐
gram by selecting the checked option at the project level (in Visual Studio, go to
Advanced Build Settings). If you then need to disable overflow checking just for
specific expressions or statements, you can do so with the unchecked operator.

```csharp
int x = int.MaxValue;
int y = unchecked (x + 1);
unchecked { int z = x + 1; }
```

`==` and `!=` test for equality and inequality of any type but always return a bool
value.

```csharp
int x = 1;
int y = 2;
int z = 1;
Console.WriteLine (x == y); // False
Console.WriteLine (x == z); // True
```

**For reference types, equality, by default, is based on reference, as opposed to the
actual value of the underlying object**

```csharp
public class Dude
{
 public string Name;
 public Dude (string n) { Name = n; }
}
...
Dude d1 = new Dude ("John");
Dude d2 = new Dude ("John");
Console.WriteLine (d1 == d2); // False
Dude d3 = d1;
Console.WriteLine (d1 == d3); // True
```

### Conditional Operators

The && and || operators test for and and or conditions. They are frequently used in
conjunction with the ! operator, which expresses not.

```csharp
static bool UseUmbrella (bool rainy, bool sunny, bool windy)
{
 return !windy && (rainy || sunny);
}
```

The && and || operators short-circuit evaluation when possible. In the preceding
example, if it is windy, the expression (rainy || sunny) is not even evaluated.
Short-circuiting is essential in allowing expressions such as the following to run
without throwing a `NullReferenceException`

**The & and | operators also test for and and or conditions. The difference is that they do not short-circuit. For this reason, they are rarely used in place of conditional operators.**

```csharp
return !windy & (rainy | sunny);
```

### Conditional operator (ternary operator)

The conditional operator (more commonly called the ternary operator because it’s
the only operator that takes three operands) has the form q ? a : b; thus, if condi‐
tion q is true, a is evaluated, else b is evaluated:

```csharp
static int Max (int a, int b)
{
 return (a > b) ? a : b;
}
```

### Strings and Characters

C#’s char type (aliasing the System.Char type) represents a Unicode character and
occupies 2 bytes (UTF-16).

```csharp
char c = 'A'; // Simple character
```

Escape sequences express characters that cannot be expressed or interpreted literally.
An escape sequence is a backslash followed by a character with a special meaning

```csharp
char newLine = '\n';
char backSlash = '\\';
```

### Escape sequence characters

```
Char Meaning Value
\' Single quote 0x0027
\" Double quote 0x0022
\\ Backslash 0x005C
\0 Null 0x0000
\a Alert 0x0007
\b Backspace 0x0008
\f Form feed 0x000C
\n Newline 0x000A
\r Carriage return 0x000D
\t Horizontal tab 0x0009
\v Vertical tab 0x000B
```

```csharp
string a = "Here's a tab:\t";
```

### String concatenation

The + operator concatenates two strings:

```csharp
string s = "a" + "b";//ab
string s = "a" + 5; // a5
```

<aside>
⚠️ Using the + operator repeatedly to build up a string is inefficient: a better solution is
to use the System.Text.StringBuilder type.

</aside>

### String interpolation

A string preceded with the $ character is called an interpolated string. Interpolated
strings can include expressions enclosed in braces

```csharp
int x = 4;
Console.Write ($"A square has {x} sides"); // Prints: A square has 4 sides
```

### Definite Assignment

C# enforces a definite assignment policy. In practice, this means that outside of an unsafe context, it’s impossible to access uninitialized memory.

- Local variables must be assigned a value before they can be read.
- Function arguments must be supplied when a method is called (unless marked as optional).
- All other variables (such as fields and array elements) are automatically initialized by the runtime.

For example, the following code results in a compile-time error

```csharp
static void Main()
{
 int x;
 Console.WriteLine (x); // Compile-time error
}
```

Fields and array elements are automatically initialized with the default values for
their type. 

```csharp
static void Main()
{
 int[] ints = new int[2];
 Console.WriteLine (ints[0]); // 0
}
```

The following code outputs 0, because fields are implicitly assigned a default value

```csharp
class Test
{
 static int x;
 static void Main() { Console.WriteLine (x); } // 0
}
```

### Default Values

- All reference types : `null`
- All numeric and enum types : `0`
- char type : `\0`
- bool type : `False`

You can obtain the default value for any type via the default keyword:

```csharp
Console.WriteLine (default (decimal)); // 0
decimal d = default;
```

### Parameters

A method may have a sequence of parameters. Parameters define the set of argu‐
ments that must be provided for that method.

You can control how parameters are passed with the ref, in, and out modifier

### The out modifier

An out argument is like a ref argument except for the following

- It need not be assigned before going into the function.
- It must be assigned before it comes out of the function.

The out modifier is most commonly used to get multiple return values back from a
method

```csharp
class Test
{
 static void Split (string name, out string firstNames,
 out string lastName)
 {
 int i = name.LastIndexOf (' ');
 firstNames = name.Substring (0, i);
 lastName = name.Substring (i + 1);
 }
 static void Main()
 {
 string a, b;
 Split ("Stevie Ray Vaughan", out a, out b);
 Console.WriteLine (a); // Stevie Ray
 Console.WriteLine (b); // Vaughan
 }
}
```

**Like a ref parameter, an out parameter is passed by reference.**

### Out variables and discards

From C# 7, you can declare variables on the fly when calling methods with out parameters. We can shorten the Main method in our preceding example as follows.

When calling methods with multiple out parameters, sometimes you’re not interested in receiving values from all the parameters. In such cases, you can discard the ones in which you’re not interested by using an underscore

```csharp
Split ("Stevie Ray Vaughan", out string a, out _); // Discard the 2nd param
Console.WriteLine (a);
```

### The in modifier

An in parameter is similar to a ref parameter except that the argument’s value can not modified by the method (doing so generates a compile-time error). This modifier is most useful when passing a large value type to the method because **it allows the compiler to avoid the overhead of copying the argument prior to passing it in while still protecting the original value from modification**.

Overloading solely on the presence of in is permitted

```csharp
void Foo ( SomeBigStruct a) { ... }
void Foo (in SomeBigStruct a) { ... }
SomeBigStruct x = ...;
Foo (x); // Calls the first overload
Foo (in x); // Calls the second overload
```

### Named arguments

```csharp
void Foo (int x, int y) { Console.WriteLine (x + ", " + y); }
void Test()
{
 Foo (x:1, y:2); // 1, 2
}
```

**Named arguments can occur in any order.** The following calls to Foo are semantically identical:

```csharp
Foo (x:1, y:2);
Foo (y:2, x:1);
```

<aside>
💡 Named arguments are particularly useful in conjunction with optional parameters.
For instance, consider the following method

</aside>

```csharp
void Bar (int a = 0, int b = 0, int c = 0, int d = 0) { ... }
Bar (d:3);
```

### var Implicitly Typed Local Variables

It is often the case that you declare and initialize a variable in one step. If the compiler is able to infer the type from the initialization expression, you can use the keyword var in place of the type declaration.

```csharp
var x = "hello";
var y = new System.Text.StringBuilder();
var z = (float)Math.PI;
```

### Null Operators

### Null-Coalescing Operator

The `??` operator is the null-coalescing operator. It says, “If the operand to the left is non-null, give it to me; otherwise, give me another value.”

```csharp
string s1 = null;
string s2 = s1 ?? "nothing"; // s2 evaluates to "nothing"
```

### Null-Coalescing Assignment Operator

The `??=` operator is the null-coalescing assignment operator. It says, “If the operand to the left is null, assign the right operand to the left operand.”

```csharp
string s1 = null;
s1 ??= "something";
Console.WriteLine (s1); // something
s1 ??= "everything";
Console.WriteLine (s1); // something
```

### Null-Conditional Operator

The `?.` operator is the null-conditional or “Elvis” operator (after the Elvis emoticon).
It allows you to call methods and access members just like the standard dot operator
except that if the operand on the left is null, the expression evaluates to null instead
of throwing a NullReferenceException

```csharp
System.Text.StringBuilder sb = null;
string s = sb?.ToString(); // No error; s instead evaluates to null
//The last line is equivalent to the following
string s = (sb == null ? null : sb.ToString());
```

### Namespaces

A namespace is a domain for type names. Types are typically organized into hier‐
archical namespaces, making them easier to find and avoiding conflicts.

- Namespaces are independent of assemblies, which are units of
deployment such as an .exe or .dll
- Namespaces also have no impact on member visibility—
public, internal, private, and so on.

```csharp
namespace Outer.Middle.Inner
{
 class Class1 {}
 class Class2 {}
}
```

The dots in the namespace indicate a hierarchy of nested namespaces. The code that follows is semantically identical to the preceding example

```csharp
namespace Outer
{
 namespace Middle
 {
 namespace Inner
 {
 class Class1 {}
 class Class2 {}
 }
 }
}
```

### Repeated namespaces

```csharp
namespace Outer.Middle.Inner
{
 class Class1 {}
}
namespace Outer.Middle.Inner
{
 class Class2 {}
}
```

We can even break the example into two source files such that we could compile
each class into a different assembly.

```csharp
//Source file 1:
namespace Outer.Middle.Inner
{
class Class1 {}
}
//Source file 2:
namespace Outer.Middle.Inner
{
 class Class2 {}
}
```# Chapter 20 : Dynamic Programming

In this chapter, we look briefly at the Dynamic Language Runtime and then explore the following dynamic programming patterns:

# The Dynamic Language Runtime

C# relies on the Dynamic Language Runtime (DLR) to perform dynamic binding

Contrary to its name, the **DLR is not a dynamic version of the CLR. Rather, it’s a library that sits atop the CLR**—just like any other library such as System.Xml.dll. Its primary role is to provide runtime services to unify dynamic programming—in both statically and dynamically typed languages. Hence, languages such as C#, Visual Basic, IronPython, and IronRuby all use the same protocol for calling functions dynamically. This allows them to share libraries and call code written in other languages.

# Numeric Type Unification

```csharp
static dynamic Mean (dynamic x, dynamic y) => (x + y) / 2;
static void Main()
{
 int x = 3, y = 5;
 Console.WriteLine (Mean (x, y));
}
```

However, this (unnecessarily) sacrifices static type safety. The following compiles without error but then fails at runtime:

```csharp
string s = Mean (3, 5); // Runtime error!
```

We can fix this by introducing a generic type parameter and then casting to dynamic within the calculation itself:

```csharp
static T Mean<T> (T x, T y)
{
 dynamic result = ((dynamic) x + y) / 2;
 return (T) result;
}
```

Dynamic binding incurs a small performance hit—even with call-site caching. You can mitigate this by adding statically typed overloads that cover just the most com‐ monly used types. For example, if subsequent performance profiling showed that calling Mean with doubles was a bottleneck, you could add the following overload:

```csharp
static double Mean (double x, double y) => (x + y) / 2;
```

The compiler will favor that overload when Mean is called with arguments that are known at compile time to be of type double.

# Implementing Dynamic Objects

An object can provide its binding semantics by implementing IDynamicMetaObject Provider—or more easily by subclassing DynamicObject, which provides a default implementation of this interface. This is demonstrated briefly in Chapter 4 via the following example:

```csharp
static void Main()
{
 dynamic d = new Duck();
 d.Quack(); // Quack method was called
 d.Waddle(); // Waddle method was called
}
public class Duck : DynamicObject
{
 public override bool TryInvokeMember (
 InvokeMemberBinder binder, object[] args, out object result)
 {
 Console.WriteLine (binder.Name + " method was called");
 result = null;
 return true;
 }
}
```

# Interoperating with Dynamic Languages

Although C# supports dynamic binding via the dynamic keyword, it doesn’t go as far as allowing you to execute an expression described in a string at runtime:

```csharp
string expr = "2 * 3";
// We can't "execute" expr
```

This is because the code to translate a string into an expression tree requires a lexi‐ cal and semantic parser. These features are built into the C# compiler and are not available as a runtime service. At runtime, C# merely provides a binder, which instructs the DLR how to interpret an already built expression tree.

True dynamic languages such as IronPython and IronRuby do allow you to execute an arbitrary string, and this is useful in tasks such as scripting, dynamic configura‐ tion, and implementing dynamic rules engines. So, although you can write most of your application in C#, it can be useful to call out to a dynamic language for such tasks. In addition, you might want to use an API that is written in a dynamic lan‐ guage where no equivalent functionality is available in a .NET library

In the following example, we use IronPython to evaluate an expression created at runtime from within C#. You could use this script to write a calculator:

```csharp
using System;
using IronPython.Hosting;
using Microsoft.Scripting;
using Microsoft.Scripting.Hosting;
class Calculator
{
 static void Main()
 {
 int result = (int) Calculate ("2 * 3");
 Console.WriteLine (result); // 6
 }
 static object Calculate (string expression)
 {
 ScriptEngine engine = Python.CreateEngine();
 return engine.Execute (expression);
 }
}
```

To run this code, add the NuGet packages DynamicLanguageRuntime (not to be confused with the System.Dynamic.Run‐ time package) and IronPython to your application.

Because we’re passing a string into Python, the expression will be evaluated accord‐ ing to Python’s rules and not C#’s. It also means that we can use Python’s language features, such as lists:

```csharp
var list = (IEnumerable) Calculate ("[1, 2, 3] + [4, 5]");
foreach (int n in list) Console.Write (n); // 12345
```# Chapter 25 : Native and COM Interoperability

This chapter describes how to integrate with native (unmanaged) Dynamic-Link Libraries (DLLs) and Component Object Model (COM) components. Unless other‐ wise stated, the types mentioned in this chapter exist in either the System or the `System.Runtime.InteropServices` namespace.

# COM Interoperability

The .NET runtime provides special support for COM, enabling COM objects to be used from .NET, and vice versa. COM is available only on Windows.

## The Purpose of COM

COM is an acronym for **Component Object Model**, a binary standard for interfacing with libraries, released by Microsoft in 1993. The **motivation for inventing COM was to enable components to communicate with one another in a language independent and version-tolerant manner.** Before COM, the approach in Windows was to publish DLLs that declared structures and functions using the C programming language. Not only is this approach language specific, but it’s also brittle. The specification of a type in such a library is inseparable from its implementation: even updating a structure with a new field means breaking its specification.

The beauty of COM was to separate the specification of a type from its underlying implementation through a construct known as a COM interface. COM also allowed for the calling of methods on stateful objects—rather than being limited to simple procedure calls.

**In a way, the .NET programming model is an evolution of the principles of COM programming: the .NET platform also facilitates cross-language development and allows binary components to evolve without breaking applications that depend on them.**

## The Basics of the COM Type System

The COM type system revolves around interfaces. A COM interface is rather like a .NET interface, but it’s more prevalent because a COM type exposes its functional‐ ity only through an interface. In the .NET world, for instance, we could declare a type simply, as follows:

```csharp
public class Foo
{
 public string Test() => "Hello, world";
}
```

Consumers of that type can use Foo directly. And if we later changed the implementation of Test(), calling assemblies would not require recompilation. In this respect, .NET separates interface from implementation—without requiring inter‐ faces. We could even add an overload without breaking callers:

```csharp
public string Test (string s) => $"Hello, world {s}";
```

In the COM world, Foo exposes its functionality through an interface to achieve this same decoupling. So, in Foo’s type library, an interface such as this would exist

```csharp
public interface IFoo { string Test(); }
```

(We’ve illustrated this by showing a C# interface—not a COM interface. The princi‐ ple, however, is the same—although the plumbing is different.)

When it comes to adding the overloaded version of Test, life is more complicated with COM than with .NET. First, we would avoid modifying the IFoo interface because this would break binary compatibility with the previous version (one of the principles of COM is that interfaces, once published, are immutable). Second, COM doesn’t allow method overloading. The solution is to instead have Foo implement a second interface:

```csharp
public interface IFoo2 { string Test (string s); }
//(Again, we’ve transliterated this into a .NET interface for familiarity.)
```

Supporting multiple interfaces is of key importance in making COM libraries versionable

## IUnknown and IDispatch

All COM interfaces are identified with a Globally Unique Identifier (GUID).

The root interface in COM is IUnknown all COM objects must implement it. This interface has three methods:

- AddRef
- Release
- QueryInterface

AddRef and Release are for lifetime management given that COM uses reference counting rather than automatic garbage collection (COM was designed to work with unmanaged code, where automatic garbage collection isn’t feasible). The Query Interface method returns an object reference that supports that interface, if it can do so.

To enable dynamic programming (e.g., scripting and automation), a COM object can also implement IDispatch. This enables dynamic languages such as VBScript to call COM objects in a late-bound manner—rather like dynamic in C# (although only for simple invocations).

# Calling a COM Component from C#

The CLR’s built-in support for COM means that you don’t work directly with IUnknown and IDispatch. Instead, you work with CLR objects and the runtime marshals your calls to the COM world via Runtime-Callable Wrappers (RCWs). The runtime also handles lifetime management by calling AddRef and Release (when the .NET object is finalized) and takes care of the primitive type conversions between the two worlds. Type conversion ensures that each side sees, for example, the integer and string types in their familiar forms.

The CLR’s built-in support for COM means that you don’t work directly with IUnknown and IDispatch. Instead, you work with CLR objects and the runtime marshals your calls to the COM world via Runtime-Callable Wrappers (RCWs). The runtime also handles lifetime management by calling AddRef and Release (when the .NET object is finalized) and takes care of the primitive type conversions between the two worlds. Type conversion ensures that each side sees, for example, the integer and string types in their familiar forms.

Additionally, there needs to be some way to access RCWs in a statically typed fash‐ ion. This is the job of COM interop types. COM interop types are automatically gen‐ erated proxy types that expose a .NET member for each COM member. The type library importer tool (tlbimp.exe) generates COM interop types from the command line, based on a COM library that you choose, and compiles them into a COM interop assembly.

If a COM component implements multiple interfaces, the tlbimp.exe tool generates a single type that contains a union of members from all interfaces.

You can create a COM interop assembly in Visual Studio by going to the Add Refer‐ ence dialog box and choosing a library from the COM tab. For example, if you have Microsoft Excel installed, adding a reference to the Microsoft Excel Object Library allows you to interoperate with Excel’s COM classes. Here’s the C# code to create and show a workbook, and then populate a cell in that workbook:

```csharp
using System;
using Excel = Microsoft.Office.Interop.Excel;
class Program
{
 static void Main()
 {
 var excel = new Excel.Application();
 excel.Visible = true;
 Excel.Workbook workBook = excel.Workbooks.Add();
 ((Excel.Range)excel.Cells[1, 1]).Font.FontStyle = "Bold";
 ((Excel.Range)excel.Cells[1, 1]).Value2 = "Hello World";
 workBook.SaveAs (@"d:\temp.xlsx");
 }
}
```

**It is currently necessary to embed interop types in your application (otherwise, .NET Core won’t locate them at runtime).** Either click the COM reference in Visual Studio’s Solution Explorer and set the Embed Interop Types property to true in the Properties window, or open your .csproj file and add the following line (in boldface):

```csharp
<ItemGroup>
 <COMReference Include="Microsoft.Office.Excel.dll">
 ...
 <EmbedInteropTypes>true</EmbedInteropTypes>
 </COMReference>
</ItemGroup>
```

### Optional Parameters and Named Arguments

Because COM APIs don’t support function overloading, it’s very common to have functions with numerous parameters, many of which are optional. For instance, here’s how you might call an Excel workbook’s Save method:

```csharp
var missing = System.Reflection.Missing.Value;
workBook.SaveAs (@"d:\temp.xlsx", missing, missing, missing, missing,
 missing, Excel.XlSaveAsAccessMode.xlNoChange, missing, missing,
 missing, missing, missing);

//The good news is that C#’s support for optional parameters is COM-aware, so we
//can just do this:
workBook.SaveAs (@"d:\temp.xlsx");

//Named arguments allow you to specify additional arguments, regardless of their
//position:
workBook.SaveAs (@"c:\test.xlsx", Password:"foo");
```

## Implicit ref Parameters

Some COM APIs (Microsoft Word, in particular) expose functions that declare every parameter as pass-by-reference—whether or not the function modifies the parameter value. This is because of the perceived performance gain from not copying argument values (the real performance gain is negligible).

Historically, calling such methods from C# has been clumsy because you must specify the ref keyword with every argument, and this prevents the use of optional parameters. For instance, to open a Word document, we used to have to do this:

```csharp
object filename = "foo.doc";
object notUsed1 = Missing.Value;
object notUsed2 = Missing.Value;
object notUsed3 = Missing.Value;
...
Open (ref filename, ref notUsed1, ref notUsed2, ref notUsed3, ...);
```

Thanks to implicit ref parameters, you can omit the ref modifier on COM function calls, allowing the use of optional parameters

```csharp
word.Open ("foo.doc");
```

The caveat is that you will get neither a compile-time nor a runtime error if the COM method you’re calling actually does mutate an argument value.

## Indexers

The ability to omit the ref modifier has another benefit: it makes COM indexers with ref parameters accessible via ordinary C# indexer syntax. This would other‐ wise be forbidden because ref/out parameters are not supported with C# indexers

You can also call COM properties that accept arguments. In the following example, Foo is a property that accepts an integer argument:

```csharp
myComObject.Foo [123] = "Hello";
```

Writing such properties yourself in C# is still prohibited: a type can expose an indexer only on itself (the default indexer). Therefore, if you wanted to write code in C# that would make the preceding statement legal, Foo would need to return another type that exposed a (default) indexer.

## Dynamic Binding

There are two ways that dynamic binding can help when calling COM components. The first way is in allowing access to a COM component without a COM interop type. To do this, call Type.GetTypeFromProgID with the COM component name to obtain a COM instance, and then use dynamic binding to call members from then on. Of course, there’s no IntelliSense, and compile-time checks are impossible:

```csharp
Type excelAppType = Type.GetTypeFromProgID ("Excel.Application", true);
dynamic excel = Activator.CreateInstance (excelAppType);
excel.Visible = true;
dynamic wb = excel.Workbooks.Add();
excel.Cells [1, 1].Value2 = "foo";
```

Dynamic binding can also be useful (to a lesser extent) in dealing with the COM variant type. For reasons due more to poor design that necessity, COM API functions are often peppered with this type, which is roughly equivalent to object in .NET. If you enable “Embed Interop Types” in your project (more on this soon), the runtime will map variant to dynamic, instead of mapping variant to object, avoiding the need for casts. For instance, you could legally do this:

```csharp
excel.Cells [1, 1].Font.FontStyle = "Bold";
```

# Embedding Interop Types

We said previously that C# ordinarily calls COM components via interop types that are generated by calling the tlbimp.exe tool (directly, or via Visual Studio).

Historically, your only option was to reference interop assemblies just as you would with any other assembly. This could be troublesome because interop assemblies can get quite large with complex COM components. A tiny add-in for Microsoft Word, for instance, requires an interop assembly that is orders of magnitude larger than itself.

Rather than referencing an interop assembly, you have the option of embedding the portions that use it. The compiler analyzes the assembly to work out precisely the types and members that your application requires, and embeds definitions for (just) those types and members directly in your application. This avoids bloat as well as the need to ship an additional file.

To enable this feature, either select the COM reference in Visual Studio’s Solution Explorer and then set Embed Interop Types to true in the Properties window, or edit your .csproj file as we described earlier

## Type Equivalence

The CLR supports type equivalence for linked interop types. This means that if two assemblies each link to an interop type, those types will be considered equivalent if they wrap the same COM type. This holds true even if the interop assemblies to which they linked were generated independently.

Type equivalence relies on the TypeIdentifierAttribute attribute in the System.Runtime.InteropServices name‐ space. The compiler automatically applies this attribute when you link to interop assemblies. COM types are then consid‐ ered equivalent if they have the same GUID.

# Exposing C# Objects to COM

It’s also possible to write classes in C# that can be consumed in the COM world. The CLR makes this possible through a proxy called a COM-Callable Wrapper (CCW). A CCW marshals types between the two worlds (as with an RCW) and implements IUnknown (and optionally IDispatch) as required by the COM protocol.A CCW is lifetime-controlled from the COM side via reference counting (rather than through the CLR’s garbage collector).

You can expose any public class to COM (as an in-proc server). To do so, first create an interface, assign it a unique GUID (in Visual Studio, you can use Tools > Create GUID), declare it visible to COM, and then set the interface type:

```csharp
namespace MyCom
{
 [ComVisible(true)]
 [Guid ("226E5561-C68E-4B2B-BD28-25103ABCA3B1")] // Change this GUID
 [InterfaceType (ComInterfaceType.InterfaceIsIUnknown)]
 public interface IServer
 {
 int Fibonacci();
 }
}
```

Next, provide an implementation of your interface, assigning a unique GUID to that implementation:

```csharp
namespace MyCom
{
 [ComVisible(true)]
 [Guid ("09E01FCD-9970-4DB3-B537-0EC555967DD9")] // Change this GUID
 public class Server
 {
 public ulong Fibonacci (ulong whichTerm)
 {
 if (whichTerm < 1) throw new ArgumentException ("...");
 ulong a = 0;
 ulong b = 1;
 for (ulong i = 0; i < whichTerm; i++)
 {
 ulong tmp = a;
 a = b;
 b = tmp + b;
 }
 return a;
 }
 }
}
```

Edit your .csproj file, adding the following line (in boldface):

```xml
<PropertyGroup>
 <TargetFramework>netcoreapp3.0</TargetFramework>
 **<EnableComHosting>true</EnableComHosting>**
</PropertyGroup>
```

Now, when you build your project, an additional file is generated, `MyCom.comhost.dll`, which can be registered for COM interop. (Keep in mind that the file will always be 32 bit or 64 bit depending on your project configuration: there’s no such thing as “Any CPU” in this scenario.) From an elevated command prompt, switch to the directory holding your DLL and run regsvr32 `MyCom.comhost.dll`.

You can then consume your COM component from most COM-capable languages. For example, you can create this Visual Basic Script in a text editor and run it by double-clicking the file in Windows Explorer, or by starting it from a command prompt as you would a program:

```visual-basic
REM Save file as ComClient.vbs
Dim obj
Set obj = CreateObject("MyCom.Server")
result = obj.Fibonacci(12)
Wscript.Echo result
```

Note that .NET Framework and .NET Core cannot be loaded into the same process. Therefore, a .NET Core COM server cannot be loaded into a .NET Framework COM client process, or vice versa.

## Enabling Registry-Free COM

Traditionally, COM adds type information to the registry. Registry-free COM uses a manifest file instead of the registry to control object activation. To enable this feature, add the following line (in boldface) to your .csproj file:

```xml
<PropertyGroup>
 <TargetFramework>netcoreapp3.0</TargetFramework>
 <EnableComHosting>true</EnableComHosting>
 **<EnableRegFreeCom>true</EnableRegFreeCom>**
</PropertyGroup>
```

Your build will then generate MyCom.X.manifest

There is no support in .NET Core 3 for generating a COM type library (*.tlb). You can manually write an Interface Definition Language (IDL) file or C++ header for the native declarations in your interface.# Chapter 26 : Regular Expressions

# Use Case

Regular expressions are used for tasks such as:

- Validating text input such as passwords and phone numbers
- Parsing textual data into more structured forms (e.g., a NuGet version string)
- Replacing patterns of text in a document (e.g., whole words only)

All regular expression types are defined in `System.Text.RegularExpressions`

<aside>
🔥 All inputs in Excel add-ins are in text format, so we ca use Regular Expression for data validation or for parsing.

</aside>

# Regular Expression Basics

One of the most common regular expression operators is a quantifier. ? is a quanti‐ fier that matches the preceding item 0 or 1 time. In other words, ? means optional. An item is either a single character or a complex structure of characters in square brackets. For example, the regular expression "colou?r" matches color and colour, but not colouur:

```csharp
Console.WriteLine (Regex.Match ("color", @"colou?r").Success); // True
Console.WriteLine (Regex.Match ("colour", @"colou?r").Success); // True
Console.WriteLine (Regex.Match ("colouur", @"colou?r").Success); // False
```

Regex.Match searches within a larger string. The object that it returns has proper‐ ties for the Index and Length of the match as well as the actual Value matched:

```csharp
Match m = Regex.Match ("any colour you like", @"colou?r");
Console.WriteLine (m.Success); // True
Console.WriteLine (m.Index); // 4
Console.WriteLine (m.Length); // 6
Console.WriteLine (m.Value); // colour
Console.WriteLine (m.ToString()); // colour
```

You can think of `Regex.Match` as a more powerful version of the string’s `IndexOf` method. The difference is that it searches for a pattern rather than a literal string. 

The `IsMatch` method is a shortcut for calling Match and then testing the Success property.

The regular expressions engine works from left to right by default, so only the left‐ most match is returned. You can use the NextMatch method to return more matches:

```csharp
Match m1 = Regex.Match ("One color? There are two colours in my head!",
 @"colou?rs?");
Match m2 = m1.NextMatch();
Console.WriteLine (m1); // color
Console.WriteLine (m2); // colours
```

The Matches method returns all matches in an array. We can rewrite the preceding example as follows:

```csharp
foreach (Match m in Regex.Matches
 ("One color? There are two colours in my head!", @"colou?rs?"))
 Console.WriteLine (m);
```

Another common regular expressions operator is the alternator, expressed with a vertical bar, |. An alternator expresses alternatives. The following matches “Jen,” “Jenny,” and “Jennifer”:

```csharp
Console.WriteLine (Regex.IsMatch ("Jenny", "Jen(ny|nifer)?")); // True
```

The brackets around an alternator separate the alternatives from the rest of the expression.

<aside>
📢 You can specify a timeout when matching regular expressions. If a match operation takes longer than the specified TimeSpan, a `RegexMatchTimeoutException` is thrown. This can be useful if your program processes arbitrary regular expressions (for instance, in an advanced search dialog box) because it pre‐ vents malformed regular expressions from infinitely spinning.

</aside>

## Compiled Regular Expressions

In some of the preceding examples, we called a static RegEx method repeatedly with the same pattern. An alternative approach in these cases is to instantiate a Regex object with the pattern and `RegexOptions.Compiled` and then call instance methods:

```csharp
Regex r = new Regex (@"sausages?", RegexOptions.Compiled);
Console.WriteLine (r.Match ("sausage")); // sausage
Console.WriteLine (r.Match ("sausages")); // sausages
```

RegexOptions.Compiled instructs the RegEx instance to use lightweight code gener‐ ation (DynamicMethod in Reflection.Emit) to dynamically build and compile code tailored to that particular regular expression. This results in faster matching, at the expense of an initial compilation cost.

You can also instantiate a Regex object without using RegexOptions.Compiled. A Regex instance is immutable.

<aside>
📢 The regular expressions engine is fast. Even without compilation, a simple match typically takes less than a microsecond.

</aside>

## RegexOptions

The `RegexOptions` flags `enum` lets you tweak matching behavior. A common use for `RegexOptions` is to perform a case-insensitive search:

```csharp
Console.WriteLine (Regex.Match ("a", "A", RegexOptions.IgnoreCase)); // a
```

This applies the current culture’s rules for case equivalence. The `CultureInvariant` flag lets you request the invariant culture instead:

```csharp
Console.WriteLine (Regex.Match ("a", "A", RegexOptions.IgnoreCase
 | RegexOptions.CultureInvariant));
```

You can activate most of the `RegexOptions` flags within a regular expression itself, using a single-letter code, as follows:

```csharp
Console.WriteLine (Regex.Match ("a", @"(?i)A")); // a
```

You can turn options on and off throughout an expression:

```csharp
Console.WriteLine (Regex.Match ("AAAa", @"(?i)a(?-i)a"));
```

Another useful option is `IgnorePatternWhitespace` or (?x). This allows you to insert whitespace to make a regular expression more readable—without the white‐ space being taken literally.

## Character Escapes

Regular expressions have the following metacharacters, which have a special rather than literal meaning:

```csharp
• \ * + ? | { [ () ^ $ . #
```

To use a metacharacter literally, you must prefix, or escape, the character with a backslash. In the following example, we escape the ? character to match the string "what?":

```csharp
Console.WriteLine (Regex.Match ("what?", @"what\?")); // what? (correct)
Console.WriteLine (Regex.Match ("what?", @"what?")); // what (incorrect)
```

The Regex’s Escape and Unescape methods convert a string containing regular expression metacharacters by replacing them with escaped equivalents, and vice versa:

```csharp
Console.WriteLine(Regex.Escape(@"?")); // \?
Console.WriteLine(Regex.Unescape(@"\?")); // ?
```

All the regular expression strings in this chapter we express with the C# @ literal. This is to bypass C#’s escape mechanism, which also uses the backslash. Without the @, a literal backslash would require four backslashes:

```csharp
Console.WriteLine (Regex.Match ("\\", "\\\\")); // \
```

Unless you include the (?x) option, spaces are treated literally in regular expressions:

```csharp
Console.Write (Regex.IsMatch ("hello world", @"hello world")); // True
```

## Character Sets

Character sets act as wildcards for a particular set of characters:

To match exactly one of a set of characters, put the character set in square brackets:

```csharp
Console.Write (Regex.Matches ("That is that.", "[Tt]hat").Count); // 2
```

To match any character except those in a set, put the set in square brackets with a ^ symbol before the first character:

```csharp
Console.Write (Regex.Match ("quiz qwerty", "q[^aeiou]").Index); // 5
```

You can specify a range of characters by using a hyphen. The following regular expression matches a chess move:

```csharp
Console.Write (Regex.Match ("b1-c4", @"[a-h]\d-[a-h]\d").Success); // True
```

**`\d` indicates a digit character, so `\d` will match any digit. `\D` matches any non-digit character.**

\w indicates a word character, which includes letters, numbers, and the underscore. \W matches any nonword character. These work as expected for non-English letters, too, such as Cyrillic.

`.` matches any character except \n (but allows \r).

`\p` matches a character in a specified category, such as `{Lu}` for uppercase letter or `{P}` for punctuation (we list the categories in the reference section later in the chapter):

```csharp
Console.Write (Regex.IsMatch ("Yes, please", @"\p{P}")); // True
```

We will find more uses for `\d`, `\w`, and `.` when we combine them with quantifiers.

## Quantifiers

Quantifiers match an item a specified number of times:

The * quantifier matches the preceding character or group zero or more times. The following matches cv.docx, along with any numbered versions of the same file (e.g., cv2.docx, cv15.docx):

```csharp
Console.Write (Regex.Match ("cv15.docx", @"cv\d*\.docx").Success); // True
```

Notice that we must escape the period in the file extension using a backslash.

The following allows anything between cv and .docx and is equivalent to dir cv*.docx:

```csharp
Console.Write (Regex.Match ("cvjoint.docx", @"cv.*\.docx").Success); // True
```

The + quantifier matches the preceding character or group one or more times; for example:

```csharp
Console.Write (Regex.Matches ("slow! yeah slooow!", "slo+w").Count); // 2
```

The {} quantifier matches a specified number (or range) of repetitions. The follow‐ ing matches a blood pressure reading:

```csharp
Regex bp = new Regex (@"\d{2,3}/\d{2,3}");
Console.WriteLine (bp.Match ("It used to be 160/110")); // 160/110
Console.WriteLine (bp.Match ("Now it's only 115/75")); // 115/75
```

## Greedy Versus Lazy Quantifiers

By default, quantifiers are greedy, as opposed to lazy. A greedy quantifier repeats as many times as it can before advancing. A lazy quantifier repeats as few times as it can before advancing. You can make any quantifier lazy by suffixing it with the ? symbol. To illustrate the difference, consider the following HTML fragment:

```csharp
string html = "<i>By default</i> quantifiers are <i>greedy</i> creatures";
```

Suppose that we want to extract the two phrases in italics. If we execute the following:

```csharp
foreach (Match m in Regex.Matches (html, @"<i>.*</i>"))
 Console.WriteLine (m);
```

the result is not two matches, but a single match:

```csharp
<i>By default</i> quantifiers are <i>greedy</i>
```

The problem is that our `*` quantifier greedily repeats as many times as it can before matching `</i>`. So, it passes right by the first `</i>`, stopping only at the final `</i>` (the last point at which the rest of the expression can still match).

If we make the quantifier lazy, the `*` bails out at the first point at which the rest of the expression can match:

```csharp
foreach (Match m in Regex.Matches (html, @"<i>.*?</i>"))
 Console.WriteLine (m);
```

Here’s the result:

```csharp
<i>By default</i>
<i>greedy</i>
```

# Zero-Width Assertions

The regular expressions language lets you place conditions on what should occur before or after a match, through lookbehind, lookahead, anchors, and word boundaries. These are called zero-width assertions, because they don’t increase the width (or length) of the match itself.

## Lookahead and Lookbehind

The `(?=expr)` construct checks whether the text that follows matches expr, without including expr in the result. This is called positive lookahead. In the following example, we look for a number followed by the word miles:

```csharp
Console.WriteLine (Regex.Match ("say 25 miles more", @"\d+\s(?=miles)"));//25
```

Notice the word “miles” was not returned in the result, even though it was required to satisfy the match.

After a successful lookahead, matching continues as though the sneak preview never took place. So, if we append .* to our expression like this:

```csharp
Console.WriteLine (Regex.Match ("say 25 miles more", @"\d+\s(?=miles).*"));
//25 miles more
```

Lookahead can be useful in enforcing rules for a strong password. Suppose that a password must be at least six characters and contain at least one digit. With a lookup, we could achieve this as follows:

```csharp
string password = "..."; bool ok = Regex.IsMatch (password, @"(?=.*\d).{6,}");
```

This first performs a lookahead to ensure that a digit occurs somewhere in the string. If satisfied, it returns to its position before the sneak preview began and matches six or more characters.

The opposite is the negative lookahead construct, (?!expr). This requires that the match not be followed by expr. The following expression matches “good”—unless “however” or “but” appears later in the string:

```csharp
string regex = "(?i)good(?!.*(however|but))";
Console.WriteLine (Regex.IsMatch ("Good work! But...", regex)); // False
Console.WriteLine (Regex.IsMatch ("Good work! Thanks!", regex)); // True
```

The (?<=expr) construct denotes positive lookbehind and requires that a match be preceded by a specified expression. The opposite construct, (?<!expr), denotes negative lookbehind and requires that a match not be preceded by a specified expression. For example, the following matches “good”—unless “however” appears earlier in the string:

```csharp
string regex = "(?i)(?<!however.*)good";
Console.WriteLine (Regex.IsMatch ("However good, we...", regex)); // False
Console.WriteLine (Regex.IsMatch ("Very good, thanks!", regex)); // True
```

## Anchors

The anchors `^` and `$` match a particular position. By default:

- `^` Matches the start of the string,`^` has two context-dependent meanings: an anchor and a character class negator.
- `$` Matches the end of the string. `$` has two context-dependent meanings: an anchor and a replacement group denoter

For example

```csharp
Console.WriteLine (Regex.Match ("Not now", "^[Nn]o")); // No
Console.WriteLine (Regex.Match ("f = 0.2F", "[Ff]$")); // F
```

When you specify `RegexOptions.Multiline` or include (?m) in the expression:

- `^` matches the start of the string or line (directly after a \n)
- `$` matches the end of the string or line (directly before a \n)

There’s a catch to using $ in multiline mode: a newline in Windows is nearly always denoted with \r\n rather than just \n. This means that for $ to be useful for Windows files, you must usually match the \r, as well, with a positive lookahead:

```csharp
(?=\r?$)
```

The positive lookahead ensures that \r doesn’t become part of the result. The following matches lines that end in ".txt":

```csharp
string fileNames = "a.txt" + "\r\n" + "b.docx" + "\r\n" + "c.txt";
string r = @".+\.txt(?=\r?$)";
foreach (Match m in Regex.Matches (fileNames, r, RegexOptions.Multiline))
 Console.Write (m + " ");
//OUTPUT: a.txt c.txt
```

The following matches all empty lines in string s:

```csharp
MatchCollection emptyLines = Regex.Matches (s, "^(?=\r?$)",RegexOptions.Multiline);
```

The following matches all lines that are either empty or contain only whitespace:

```csharp
MatchCollection blankLines = Regex.Matches (s, "^[ \t]*(?=\r?$)",RegexOptions.Multiline);
```

Because an anchor matches a position rather than a character, specifying an anchor on its own matches an empty string:

```csharp
nsole.WriteLine (Regex.Match ("x", "$").Length); // 0
```

## Word Boundaries

The word boundary assertion \b matches where word characters (\w) adjoin either:

- Nonword characters (\W)
- The beginning/end of the string (^ and $)

```csharp
foreach (Match m in Regex.Matches ("Wedding in Sarajevo", @"\b\w+\b"))
 Console.WriteLine (m);
//Wedding
//in
//Sarajevo
```

The following statements highlight the effect of a word boundary

```csharp
int one = Regex.Matches ("Wedding in Sarajevo", @"\bin\b").Count; // 1
int two = Regex.Matches ("Wedding in Sarajevo", @"in").Count; // 2
```

The next query uses positive lookahead to return words followed by “(sic)”:

```csharp
string text = "Don't loose (sic) your cool";
Console.Write (Regex.Match (text, @"\b\w+\b\s(?=\(sic\))")); // loose
```

# Groups

Sometimes, it’s useful to separate a regular expression into a series of subexpressions, or groups. For instance, consider the following regular expression that represents a US phone number such as 206-465-1918:

```csharp
\d{3}-\d{3}-\d{4}
```

Suppose that we want to separate this into two groups: area code and local number. We can achieve this by using parentheses to capture each group:

```csharp
(\d{3})-(\d{3}-\d{4})
```

We then retrieve the groups programmatically:

```csharp
Match m = Regex.Match ("206-465-1918", @"(\d{3})-(\d{3}-\d{4})");
Console.WriteLine (m.Groups[1]); // 206
Console.WriteLine (m.Groups[2]); // 465-1918
```

The zeroth group represents the entire match. In other words, it has the same value as the match’s Value:

```csharp
Console.WriteLine (m.Groups[0]); // 206-465-1918
Console.WriteLine (m); // 206-465-1918
```

Groups are part of the regular expressions language itself. This means that you can refer to a group within a regular expression. The \n syntax lets you index the group by group number n within the expression. For example, the expression (\w)ee\1 matches deed and peep. In the following example, we find all words in a string start‐ ing and ending in the same letter:

```csharp
foreach (Match m in Regex.Matches ("pop pope peep", @"\b(\w)\w+\1\b"))
 Console.Write (m + " "); // pop peep
```

The brackets around the \w instruct the regular expressions engine to store the sub‐ match in a group (in this case, a single letter) so that it can be used later. We refer to that group later using \1, meaning the first group in the expression.

## Named Groups

In a long or complex expression, it can be easier to work with groups by name rather than index. Here’s a rewrite of the previous example, using a group that we name 'letter'

```csharp
string regEx =
 @"\b" + // word boundary
 @"(?'letter'\w)" + // match first letter, and name it 'letter'
 @"\w+" + // match middle letters
 @"\k'letter'" + // match last letter, denoted by 'letter'
 @"\b"; // word boundary
foreach (Match m in Regex.Matches ("bob pope peep", regEx))
 Console.Write (m + " "); // bob peep
```

Here’s how to name a captured group:

```csharp
(?'group-name'group-expr) or (?<group-name>group-expr)
```

And here’s how to refer to a group:

```csharp
\k'group-name' or \k<group-name>
```

The following example matches a simple (non-nested) XML/HTML element by looking for start and end nodes with a matching name:

```csharp
string regFind =
 @"<(?'tag'\w+?).*>" + // lazy-match first tag, and name it 'tag'
 @"(?'text'.*?)" + // lazy-match text content, name it 'text'
 @"</\k'tag'>"; // match last tag, denoted by 'tag'

Match m = Regex.Match ("<h1>hello</h1>", regFind);
Console.WriteLine (m.Groups ["tag"]); // h1
Console.WriteLine (m.Groups ["text"]); // hello
```

Allowing for all possible variations in XML structure, such as nested elements, is more complex. The .NET regular expressions engine has a sophisticated extension called “matched balanced constructs” that can assist with nested tags—information on this is available on the internet and in Mastering Regular Expressions (O’Reilly) by Jeffrey E. F. Friedl

# Replacing and Splitting Text

The RegEx.Replace method works like string.Replace except that it uses a regular expression

The following replaces “cat” with “dog”. Unlike with string.Replace, “catapult” won’t change into “dogapult” because we match on word boundaries:

```csharp
string find = @"\bcat\b";
string replace = "dog";
Console.WriteLine (Regex.Replace ("catapult the cat", find, replace));
//OUTPUT: catapult the dog
```

The replacement string can reference the original match with the $0 substitution construct. The following example wraps numbers within a string in angle brackets:

```csharp
string text = "10 plus 20 makes 30";
Console.WriteLine (Regex.Replace (text, @"\d+", @"<$0>"));
OUTPUT: <10> plus <20> makes <30>
```

You can access any captured groups with $1, $2, $3, and so on, or ${name} for a named group. To illustrate how this can be useful, consider the regular expression in the previous section that matched a simple XML element. By rearranging the groups, we can form a replacement expression that moves the element’s content into an XML attribute:

```csharp
string regFind =
 @"<(?'tag'\w+?).*>" + // lazy-match first tag, and name it 'tag'
 @"(?'text'.*?)" + // lazy-match text content, name it 'text'
 @"</\k'tag'>"; // match last tag, denoted by 'tag'
string regReplace =
 @"<${tag}" + // <tag
 @"value=""" + // value="
 @"${text}" + // text
 @"""/>"; // "/>
Console.Write (Regex.Replace ("<msg>hello</msg>", regFind, regReplace));
//Here’s the result:
<msg value="hello"/>
```

## MatchEvaluator Delegate

Replace has an overload that takes a MatchEvaluator delegate, which is invoked per match. This allows you to delegate the content of the replacement string to C# code when the regular expressions language isn’t expressive enough:

```csharp
Console.WriteLine (Regex.Replace ("5 is less than 10", @"\d+",
 m => (int.Parse (m.Value) * 10).ToString()) );
//OUTPUT: 50 is less than 100
```

## Splitting Text

The static `Regex.Split` method is a more powerful version of the `string.Split` method, with a regular expression denoting the separator pattern. In this example, we split a string, where any digit counts as a separator:

```csharp
foreach (string s in Regex.Split ("a5b7c", @"\d"))
 Console.Write (s + " "); // a b c
```

The result, here, doesn’t include the separators themselves. You can include the sep‐ arators, however, by wrapping the expression in a positive lookahead. The following splits a camel-case string into separate words:

```csharp
foreach (string s in Regex.Split ("oneTwoThree", @"(?=[A-Z])"))
 Console.Write (s + " "); // one Two Three
```

# Cookbook Regular Expressions

## Recipes

### Matching Social Security number/phone number

```csharp
string ssNum = @"\d{3}-\d{2}-\d{4}";
Console.WriteLine (Regex.IsMatch ("123-45-6789", ssNum)); // True
string phone = @"(?x)
 ( \d{3}[-\s] | \(\d{3}\)\s? )
 \d{3}[-\s]?
 \d{4}";
Console.WriteLine (Regex.IsMatch ("123-456-7890", phone)); // True
Console.WriteLine (Regex.IsMatch ("(123) 456-7890", phone)); // True
```

### Extracting “name = value” pairs (one per line)

```csharp
Note that this starts with the multiline directive (?m):
string r = @"(?m)^\s*(?'name'\w+)\s*=\s*(?'value'.*)\s*(?=\r?$)";
string text =
 @"id = 3
 secure = true
 timeout = 30";
foreach (Match m in Regex.Matches (text, r))
 Console.WriteLine (m.Groups["name"] + " is " + m.Groups["value"]);
//id is 3 secure is true timeout is 30
```

### Strong password validation

```csharp
string r = @"(?x)^(?=.* ( \d | \p{P} | \p{S} )).{6,}";
Console.WriteLine (Regex.IsMatch ("abc12", r)); // False
Console.WriteLine (Regex.IsMatch ("abcdef", r)); // False
Console.WriteLine (Regex.IsMatch ("ab88yz", r)); // True
```

### Lines of at least 80 characters

```csharp
string r = @"(?m)^.{80,}(?=\r?$)";
string fifty = new string ('x', 50);
string eighty = new string ('x', 80);
string text = eighty + "\r\n" + fifty + "\r\n" + eighty;
Console.WriteLine (Regex.Matches (text, r).Count); // 2
```

### Parsing dates/times (N/N/N H:M:S AM/PM)

This expression handles a variety of numeric date formats—and works whether the year comes first or last. The (?x) directive improves readability by allowing white‐ space; the (?i) switches off case sensitivity (for the optional AM/PM designator). You can then access each component of the match through the Groups collection:

```csharp
string r = @"(?x)(?i)
(\d{1,4}) [./-]
 (\d{1,2}) [./-]
 (\d{1,4}) [\sT]
(\d+):(\d+):(\d+) \s? (A\.?M\.?|P\.?M\.?)?";
string text = "01/02/2008 5:20:50 PM";
foreach (Group g in Regex.Match (text, r).Groups)
 Console.WriteLine (g.Value + " ");
//01/02/2008 5:20:50 PM 01 02 2008 5 20 50 PM
```

(Of course, this doesn’t verify that the date/time is correct.)

### Matching Roman numerals

```csharp
string r =
 @"(?i)\bm*" +
 @"(d?c{0,3}|c[dm])" +
 @"(l?x{0,3}|x[lc])" +
 @"(v?i{0,3}|i[vx])" +
 @"\b";
Console.WriteLine (Regex.IsMatch ("MCMLXXXIV", r)); // True
```

### Removing repeated words

```csharp
string r = @"(?'dupe'\w+)\W\k'dupe'";
string text = "In the the beginning...";
Console.WriteLine (Regex.Replace (text, r, "${dupe}"));
//In the beginning
```

### Word count

```csharp
string r = @"\b(\w|[-'])+\b";
string text = "It's all mumbo-jumbo to me";
Console.WriteLine (Regex.Matches (text, r).Count); // 5
```

### Matching a GUID

```csharp
string r =
 @"(?i)\b" +
 @"[0-9a-fA-F]{8}\-" +
 @"[0-9a-fA-F]{4}\-" +
 @"[0-9a-fA-F]{4}\-" +
 @"[0-9a-fA-F]{4}\-" +
 @"[0-9a-fA-F]{12}" +
 @"\b";
string text = "Its key is {3F2504E0-4F89-11D3-9A0C-0305E82C3301}.";
Console.WriteLine (Regex.Match (text, r).Index); // 12
```

### Parsing an XML/HTML tag

Regex is useful for parsing HTML fragments—particularly when the document might be imperfectly formed:

```csharp
string r =
 @"<(?'tag'\w+?).*>" + // lazy-match first tag, and name it 'tag'
 @"(?'text'.*?)" + // lazy-match text content, name it 'textd'
 @"</\k'tag'>"; // match last tag, denoted by 'tag'
string text = "<h1>hello</h1>";
Match m = Regex.Match (text, r);
Console.WriteLine (m.Groups ["tag"]); // h1
Console.WriteLine (m.Groups ["text"]); // hello
```

### Splitting a camel-cased word

This requires a positive lookahead to include the uppercase separators

```csharp
string r = @"(?=[A-Z])";
foreach (string s in Regex.Split ("oneTwoThree", r))
Console.Write (s + " "); // one Two Three
```

### Obtaining a legal filename

```csharp
string input = "My \"good\" <recipes>.txt";
char[] invalidChars = System.IO.Path.GetInvalidFileNameChars();
string invalidString = Regex.Escape (new string (invalidChars));
string valid = Regex.Replace (input, "[" + invalidString + "]", "");
Console.WriteLine (valid);
//My good recipes.txt
```

### Parsing Google search terms from a web stats log

```csharp
string sample =
 "http://google.com/search?hl=en&q=greedy+quantifiers+regex&btnG=Search";
Match m = Regex.Match (sample, @"(?<=google\..+search\?.*q=).+?(?=(&|$))");
string[] keywords = m.Value.Split (
 new[] { '+' }, StringSplitOptions.RemoveEmptyEntries);
foreach (string keyword in keywords)
 Console.Write (keyword + " "); // greedy quantifiers regex
```# Chapter 3 : Creating Types in C#

### Declaring multiple fields together

For convenience, you can declare multiple fields of the same type in a comma separated list. This is a convenient way for all the fields to share the same attributes and field modifiers:

```csharp
static readonly int legs = 8,
 eyes = 2;
```

### Constants

A constant is evaluated statically at compile time and the compiler literally substitutes its value whenever used (rather like a macro in C++). A constant can be any of the built-in numeric types, bool, char, string, or an enum type.

```csharp
public class Test
{
 public const string Message = "Hello World";
}
```

A constant can serve a similar role to a static readonly field, but it is much more restrictive—both in the types you can use and in field initialization semantics. A constant also differs from a static readonly field in that the evaluation of the constant occurs at compile time; thus:

```csharp
public static double Circumference (double radius)
{
 return 2 * System.Math.PI * radius;
}
//is compiled to
public static double Circumference (double radius)
{
 return 6.2831853071795862 * radius;
}
```

### Expression-bodied methods

A method that comprises a single expression

```csharp
int Foo (int x) { return x * 2; }
int Foo (int x) => x * 2;
void Foo (int x) => Console.WriteLine (x);
```

### Local methods

method within another method

```csharp
void WriteCubes()
{
 Console.WriteLine (Cube (3));
 Console.WriteLine (Cube (4));
 Console.WriteLine (Cube (5));
 int Cube (int value) => value * value * value;
}
```

The local method (Cube, in this case) is visible only to the enclosing method (WriteCubes). This simplifies the containing type and instantly signals to anyone looking at the code that Cube is used nowhere else. Another benefit of local methods is that they can access the local variables and parameters of the enclosing method.

**Local methods can appear within other function kinds, such as property accessors, constructors, and so on. You can even put local methods inside other local methods, and inside lambda expressions that use a statement block**

### Static local methods

Adding the static modifier to a local method prevents it from seeing the local vari‐
ables and parameters of the enclosing method. This helps to reduce coupling as well
as enabling the local method to declare variables as it pleases, without risk of collid‐
ing with those in the containing method.

# Classes

### Properties

Properties look like fields from the outside, but internally they contain logic, like methods do.

A property is declared like a field but with a get/set block added.

```csharp
public class Stock
{
 decimal currentPrice; // The private "backing" field
 public decimal CurrentPrice // The public property
 {
 get { return currentPrice; }
 set { currentPrice = value; }
 }
}
```

### Automatic properties

The most common implementation for a property is a getter and/or setter that simply reads and writes to a private field of the same type as the property. An automatic property declaration instructs the compiler to provide this implementation.

```csharp
public class Stock
{
 ...
 public decimal CurrentPrice { get; set; }
}
```

### Property initializers

You can add a property initializer to automatic properties, just as with fields:

Properties with an initializer can be read-only:

```csharp
public decimal CurrentPrice { get; set; } = 123;
public int Maximum { get; } = 999;
```

### Static Constructors

A static constructor executes once per type rather than once per instance. A type can
define only one static constructor, and it must be parameterless and have the same
name as the type

```csharp
class Test
{
 static Test() { Console.WriteLine ("Type Initialized"); }
}
```

The runtime automatically invokes a static constructor just prior to the type being
used. Two things trigger this.

- Instantiating the type
- Accessing a static member in the type

### Static constructors and field initialization order

Static field initializers run just before the static constructor is called. If a type has no static constructor, static field initializers will execute just prior to the type being used or anytime earlier at the whim of the runtime

Static field initializers run in the order in which the fields are declared. The following example illustrates this: X is initialized to 0 and Y is initialized to 3.

```csharp
class Foo
{
 public static int X = Y; // 0
 public static int Y = 3; // 3
}
```

### Static Classes

A class can be marked static, indicating that it must be composed solely of static members and cannot be subclassed. The `System.Console` and `System.Math` classes are good examples of static classes

### Finalizers

Finalizers are class-only methods that execute before the garbage collector reclaims the memory for an unreferenced object. The syntax for a finalizer is the name of the class prefixed with the `~` symbol:

```csharp
class Class1
{
 ~Class1()
 {
 ...
 }
}
```

You can write single-statement finalizers using expression-bodied syntax

```csharp
~Class1() => Console.WriteLine ("Finalizing");
```

### Partial Types and Methods

Partial types allow a type definition to be split—typically across multiple files. A
common scenario is for a partial class to be autogenerated from some other source
(such as a Visual Studio template or designer), and for that class to be augmented
with additional hand-authored methods:

```csharp
// PaymentFormGen.cs - auto-generated
partial class PaymentForm { ... }
// PaymentForm.cs - hand-authored
partial class PaymentForm { ... }
```

Each participant must have the partial declaration

### Partial methods

A partial type can contain partial methods. These let an autogenerated partial type provide customizable hooks for manual authoring;

```csharp
partial class PaymentForm // In auto-generated file
{
 ...
 partial void ValidatePayment (decimal amount);
}
partial class PaymentForm // In hand-authored file
{
 ...
 partial void ValidatePayment (decimal amount)
 {
 if (amount > 100)
 ...
 }
}
```

A partial method consists of two parts: a definition and an implementation. The definition is typically written by a code generator, and the implementation is typically manually authored. If an implementation is not provided, the definition of the partial method is compiled away (as is the code that calls it). This allows autogenerated code to be liberal in providing hooks without having to worry about bloat. Partial methods must be void and are implicitly private

### The nameof operator

The nameof operator returns the name of any symbol (type, member, variable, and
so on) as a string:

```csharp
int count = 123;
string name = nameof (count); // name is "count"
string name = nameof (StringBuilder.Length);//"Length"
nameof (StringBuilder) + "." + nameof (StringBuilder.Length);//StringBuilder.Length
```

# Inheritance

A class can inherit from another class to extend or customize the original class. Inheriting from a class lets you reuse the functionality in that class instead of building it from scratch. A class can inherit from only a single class, but can itself be inherited by many classes, thus forming a class hierarchy.

```csharp
public class Asset
{
 public string Name;
}
public class Stock : Asset // inherits from Asset
{
 public long SharesOwned;
}
public class House : Asset // inherits from Asset
{
 public decimal Mortgage;
}
Stock msft = new Stock { Name="MSFT", SharesOwned=1000 };
Console.WriteLine (msft.Name); // MSFT
Console.WriteLine (msft.SharesOwned); // 1000

House mansion = new House { Name="Mansion", Mortgage=250000 };
Console.WriteLine (mansion.Name); // Mansion
Console.WriteLine (mansion.Mortgage); // 250000
```

The **derived classes, Stock and House**, inherit the Name property from the **base class, Asset**.

- A derived class is also called a subclass.
- A base class is also called a superclass.

### **Polymorphism**

References are polymorphic. This means a variable of type x can refer to an object
that subclasses x.

```csharp
public static void Display (Asset asset)
{
 System.Console.WriteLine (asset.Name);
}
//This method can display both a Stock and a House, since they are both Assets
**Stock** msft = new Stock ... ;
House mansion = new House ... ;
Display (msft);
Display (mansion);
```

Polymorphism works on the basis that subclasses (Stock and House) have all the
features of their base class (Asset).

The converse, however, is not true. If Display
was modified to accept a House, you could not pass in an Asset

### Casting and Reference Conversions

An object reference can be

- Implicitly upcast to a base class reference
- Explicitly downcast to a subclass reference

Upcasting and downcasting between compatible reference types performs reference conversions: a new reference is (logically) created that points to the same object. An upcast always succeeds; a downcast succeeds only if the object is suitably typed.

**Upcasting**

An upcast operation creates a base class reference from a subclass reference.

```csharp
Stock msft = new Stock();
Asset a = msft; // Upcast

Console.WriteLine (a == msft); // True

Console.WriteLine (a.Name); // OK
Console.WriteLine (a.SharesOwned); // Error: SharesOwned undefined
```

**Downcasting**

A downcast operation creates a subclass reference from a base class reference.

```csharp
Stock msft = new Stock();
Asset a = msft; // Upcast
Stock s = (Stock)a; // Downcast
Console.WriteLine (s.SharesOwned); // <No error>
Console.WriteLine (s == a); // True
Console.WriteLine (s == msft); // True
```

```csharp
House h = new House();
Asset a = h; // Upcast always succeeds
Stock s = (Stock)a; // Downcast fails: a is not a Stock
```

**The as operator**

The as operator performs a downcast that evaluates to null (rather than throwing
an exception) if the downcast fails

```csharp
Asset a = new Asset();
Stock s = a as Stock; // s is null; no exception thrown
```

This is useful when you’re going to subsequently test whether the result is null:

```csharp
if (s != null) Console.WriteLine (s.SharesOwned);
```

**The is operator**

The is operator tests whether a reference conversion would succeed; in other words, whether an object derives from a specified class (or implements an interface). It is often used to test before downcasting.The is operator also evaluates to true if an unboxing conversion would succeed.

```csharp
if (a is Stock)
 Console.WriteLine (((Stock)a).SharesOwned);
```

**The is operator and pattern variables**

From C# 7, you can introduce a variable while using the is operator:

```csharp
if (a is Stock s)
 Console.WriteLine (s.SharesOwned);
```

This is equivalent to:

```csharp
Stock s;
if (a is Stock)
{
 s = (Stock) a;
 Console.WriteLine (s.SharesOwned);
}
```

### **Virtual Function Members**

A function marked as virtual can be overridden by subclasses wanting to provide a specialized implementation. Methods, properties, indexers, and events can all be declared virtual.

```csharp
public class Asset
{
 public string Name;
 public virtual decimal Liability => 0; // Expression-bodied property
}
//(Liability => 0 is a shortcut for { get { return 0; } }.
```

A subclass overrides a virtual method by applying the override modifier:

```csharp
public class Stock : Asset
{
 public long SharesOwned;
}
public class House : Asset
{
 public decimal Mortgage;
 public override decimal Liability => Mortgage;
}
```

### Abstract Classes and Abstract Members

A class declared as abstract can never be instantiated. Instead, only its concrete subclasses can be instantiated.

Abstract classes are able to define abstract members. Abstract members are like virtual members, except they don’t provide a default implementation. That implementation must be provided by the subclass, unless that subclass is also declared abstract.

```csharp
public abstract class Asset
{
 // Note empty implementation
 public abstract decimal NetValue { get; }
}
public class Stock : Asset
{
 public long SharesOwned;
 public decimal CurrentPrice;
 // Override like a virtual method.
 public override decimal NetValue => CurrentPrice * SharesOwned;
}
```

### new versus override

```csharp
public class BaseClass
{
 public virtual void Foo() { Console.WriteLine ("BaseClass.Foo"); }
}
public class Overrider : BaseClass
{
 public override void Foo() { Console.WriteLine ("Overrider.Foo"); }
}
public class Hider : BaseClass
{
 public new void Foo() { Console.WriteLine ("Hider.Foo"); }
}

Overrider over = new Overrider();
BaseClass b1 = over;
over.Foo(); // Overrider.Foo
b1.Foo(); // Overrider.Foo
Hider h = new Hider();
BaseClass b2 = h;
h.Foo(); // Hider.Foo
b2.Foo(); // BaseClass.Foo
```

### Sealing Functions and Classes

An overridden function member may seal its implementation with the sealed keyword to prevent it from being overridden by further subclasses. In our earlier virtual function member example, we could have sealed House’s implementation of Liability, preventing a class that derives from House from overriding Liability,as follows.

```csharp
public sealed override decimal Liability { get { return Mortgage; } }
```

You can also seal the class itself, implicitly sealing all the virtual functions, by applying the sealed modifier to the class itself. Sealing a class is more common than sealing a function member.
Although you can seal against overriding, you can’t seal a member against being hidden.

### The base Keyword

The base keyword is similar to the this keyword. It serves two essential purposes

- Accessing an overridden function member from the subclass
- Calling a base-class constructor

```csharp
public class House : Asset
{
 ...
 public override decimal Liability => base.Liability + Mortgage;
}
```

With the base keyword, we access Asset’s Liability property nonvirtually. This means we will always access Asset’s version of this property—regardless of the instance’s actual runtime type.

### Constructors and Inheritance

A subclass must declare its own constructors. The base class’s constructors are accessible to the derived class, but are never automatically inherited.

```csharp
public class Baseclass
{
 public int X;
 public Baseclass () { }
 public Baseclass (int x) { this.X = x; }
}
public class Subclass : Baseclass { }

Subclass s = new Subclass (123);//This is illegal
```

Subclass must hence “redefine” any constructors it wants to expose. In doing so, however, it can call any of the base class’s constructors with the base keyword:

```csharp
public class Subclass : Baseclass
{
 public Subclass (int x) : base (x) { }
}
```

### Constructor and field initialization order

When an object is instantiated, initialization takes place in the following order

- From subclass to base class
    - Fields are initialized
    - Arguments to base-class constructor calls are evaluated
- From base class to subclass
    - Constructor bodies execute

```csharp
public class B
{
 int x = 1; // Executes 3rd
 public B (int x)
 {
 ... // Executes 4th
 }
}
public class D : B
{
int y = 1; // Executes 1st
 public D (int x)
 : base (x + 1) // Executes 2nd
 {
 ... // Executes 5th
 }
}
```

### The object Type

**object (System.Object) is the ultimate base class for all types.** Any type can be
upcast to object.

To illustrate how this is useful, consider a general-purpose stack. A stack is a data structure based on the principle of LIFO—“Last-In First-Out.” A stack has two operations: push an object on the stack, and pop an object off the stack. Here is a simple implementation that can hold up to 10 objects

```csharp
public class Stack
{
 int position;
 object[] data = new object[10];
 public void Push (object obj) { data[position++] = obj; }
 public object Pop() { return data[--position]; }
}
```

Because Stack works with the object type, we can Push and Pop instances of any type to and from the Stack

```csharp
Stack stack = new Stack();
stack.Push ("sausage");
string s = (string) stack.Pop(); // Downcast, so explicit cast is needed
Console.WriteLine (s); // sausage
```

object is a reference type, by virtue of being a class. Despite this, value types, such as int, can also be cast to and from object, and so be added to our stack. This feature of C# is called type unification and is demonstrated here:

```csharp
stack.Push (3);
int three = (int) stack.Pop();
```

<aside>
💡 When you cast between a value type and object, the CLR must perform some special work to bridge the difference in semantics between value and reference types. This process is called boxing and unboxing

</aside>

### Boxing and Unboxing

Boxing is the act of converting a value-type instance to a reference-type instance. The reference type may be either the object class or an interface

```csharp
int x = 9;
object obj = x; // Box the int
```

Unboxing reverses the operation, by casting the object back to the original value type

```csharp
int y = (int)obj; // Unbox the int
object obj = 9; // 9 is inferred to be of type int
long x = (long) obj; // InvalidCastException
```

Unboxing requires an explicit cast. The runtime checks that the stated value type
matches the actual object type, and throws an InvalidCastException if the check
fails.

### The GetType Method and typeof Operator

All types in C# are represented at runtime with an instance of System.Type. There
are two basic ways to get a System.Type object:

- Call GetType on the instance
- Use the typeof operator on a type name

GetType is evaluated at runtime; typeof is evaluated statically at compile time

```csharp
using System;
public class Point { public int X, Y; }
class Test
{
 static void Main()
 {
 Point p = new Point();
 Console.WriteLine (p.GetType().Name); // Point
 Console.WriteLine (typeof (Point).Name); // Point
 Console.WriteLine (p.GetType() == typeof(Point)); // True
 Console.WriteLine (p.X.GetType().Name); // Int32
 Console.WriteLine (p.Y.GetType().FullName); // System.Int32
 }
}
```

### The ToString Method

The ToString method returns the default textual representation of a type instance.
This method is overridden by all built-in types. Here is an example of using the int
type’s ToString method

```csharp
int x = 1;
string s = x.ToString(); // s is "1"
```

You can override the ToString method on custom types as follows

```csharp
public class Panda
{
 public string Name;
 public override string ToString() => Name;
}
...
Panda p = new Panda { Name = "Petey" };
Console.WriteLine (p); // Petey
```

**If you don’t override ToString, the method returns the type name**

# Structs

A struct is similar to a class, with the following key differences

- A struct is a value type, whereas a class is a reference type
- A struct does not support inheritance (

A struct can have all the members a class can, except the following

- A parameterless constructor
- Field initializers
- A finalizer
- Virtual or protected members

The construction semantics of a struct are as follows

- A parameterless constructor that you can’t override implicitly exists. This per‐
forms a bitwise-zeroing of its fields.
- When you define a struct constructor, you must explicitly assign every field.

```csharp
public struct Point
{
 int x, y;
 public Point (int x, int y) { this.x = x; this.y = y; }
}
```

# Access Modifiers

To promote encapsulation, a type or type member may limit its accessibility to other types and other assemblies by adding one of five access modifiers to the declaration.

1. **public**
    - Fully accessible. This is the implicit accessibility for members of an enum or
    interface.
2. **internal**
    - Accessible only within the containing assembly or friend assemblies. This is the
    default accessibility for non-nested types.
3. **private**
    - Accessible only within the containing type. This is the default accessibility for
    members of a class or struct.
4. **protected**
    - Fully accessible. This is the implicit accessibility for members of an enum or
    interface.
5. **protected internal**
    - The union of protected and internal accessibility. Eric Lippert explains it as
    follows: Everything is as private as possible by default, and each modifier
    makes the thing more accessible. So something that is protected internal is
    made more accessible in two ways.

**Examples**

```csharp
//Class2 is accessible from outside its assembly; Class1 is not:
class Class1 {} // Class1 is internal (default)
public class Class2 {}
```

```csharp
//ClassB exposes field x to other types in the same assembly; ClassA does not:
class ClassA { int x; } // x is private (default)
class ClassB { internal int x; }
```

```csharp
//Functions within Subclass can call Bar but not Foo
class BaseClass
{
 void Foo() {} // Foo is private (default)
 protected void Bar() {}
}
class Subclass : BaseClass
{
 void Test1() { Foo(); } // Error - cannot access Foo
 void Test2() { Bar(); } // OK
}
```

# Interfaces

An interface is similar to a class, but it provides a specification rather than an implementation for its members. An interface is special in the following ways:

- Interface members are all implicitly abstract. In contrast, a class can provide both abstract members and concrete members with implementations.
- A class (or struct) can implement multiple interfaces. In contrast, a class can inherit from only a single class, and a struct cannot inherit at all

An interface declaration is like a class declaration, but it provides no implementation for its members, since all its members are implicitly abstract. These members will be implemented by the classes and structs that implement the interface. A

```csharp
public interface IEnumerator
{
 bool MoveNext();
 object Current { get; }
 void Reset();
}

internal class Countdown : IEnumerator
{
 int count = 11;
 public bool MoveNext() => count-- > 0;
 public object Current => count;
 public void Reset() { throw new NotSupportedException(); }
}

IEnumerator e = new Countdown();
while (e.MoveNext())
 Console.Write (e.Current); // 109876543210
```

### Extending an Interface

Interfaces may derive from other interfaces.

```csharp
public interface IUndoable { void Undo(); }
public interface IRedoable : IUndoable { void Redo(); }
//IRedoable “inherits” all the members of IUndoable. I
```

### Explicit Interface Implementation

Implementing multiple interfaces can sometimes result in a collision between member signatures. You can resolve such collisions by explicitly implementing an interface member.

```csharp
interface I1 { void Foo(); }
interface I2 { int Foo(); }
public class Widget : I1,I2
{
 public void Foo()
 {
 Console.WriteLine ("Widget's implementation of I1.Foo");
 }
 int I2.Foo()
 {
 Console.WriteLine ("Widget's implementation of I2.Foo");
 return 42;
 }
}
```

### Reimplementing an Interface in a Subclass

A subclass can reimplement any interface member already implemented by a base class. Reimplementation hijacks a member implementation (when called through the interface) and works whether or not the member is virtual in the base class.

```csharp
public interface IUndoable { void Undo(); }
public class TextBox : IUndoable
{
 void IUndoable.Undo() => Console.WriteLine ("TextBox.Undo");
}
public class RichTextBox : TextBox, IUndoable
{
 public void Undo() => Console.WriteLine ("RichTextBox.Undo");
}

RichTextBox r = new RichTextBox();
r.Undo(); // RichTextBox.Undo Case 1
((IUndoable)r).Undo(); // RichTextBox.Undo Case 2
```

### Writing a Class Versus an Interface

As a guideline

- Use classes and subclasses for types that naturally share an implementation
- Use interfaces for types that have independent implementations

```csharp
abstract class Animal {}
abstract class Bird : Animal {}
abstract class Insect : Animal {}
abstract class FlyingCreature : Animal {}
abstract class Carnivore : Animal {}

// Concrete classes:
class Ostrich : Bird {}
class Eagle : Bird, FlyingCreature, Carnivore {} // Illegal
class Bee : Insect, FlyingCreature {} // Illegal
class Flea : Insect, Carnivore {} // Illegal
```

The Eagle, Bee, and Flea classes do not compile because inheriting from multiple classes is prohibited. To resolve this, we must convert some of the types to interfaces.

```csharp
abstract class Animal {}
abstract class Bird : Animal {}
abstract class Insect : Animal {}

interface IFlyingCreature {}
interface ICarnivore {}

// Concrete classes:
class Ostrich : Bird {}
class Eagle : Bird, IFlyingCreature, ICarnivore {} 
class Bee : Insect, IFlyingCreature {}  
class Flea : Insect, ICarnivore {}  
```

# Enums

An enum is a special value type that lets you specify a group of named numeric constants.

```csharp
public enum BorderSide { Left, Right, Top, Bottom }

//We can use this enum type as follows:

BorderSide topSide = BorderSide.Top;
bool isTop = (topSide == BorderSide.Top); // true
```

Each enum member has an underlying integral value. By default

- Underlying values are of type int.
- The constants 0, 1, 2... are automatically assigned, in the declaration order of the enum members

```csharp
//You may specify an alternative integral type, as follows:
public enum BorderSide : byte { Left, Right, Top, Bottom }
//You may also specify an explicit underlying value for each enum member:
public enum BorderSide : byte { Left=1, Right=2, Top=10, Bottom=11 }
```

### Enum Conversions

You can convert an enum instance to and from its underlying integral value with an explicit cast

```csharp
int i = (int) BorderSide.Left;
BorderSide side = (BorderSide) i;
bool leftOrRight = (int) side <= 2;
```

The numeric literal 0 is treated specially by the compiler in an enum expression and
does not require an explicit cast

```csharp
BorderSide b = 0; // No cast required
if (b == 0) ...
```

There are two reasons for the special treatment of 0

- The first member of an enum is often used as the “default” value
- For combined enum types, 0 means “no flags.”

### Flags Enums

You can combine enum members. To prevent ambiguities, members of a combinable enum require explicitly assigned values, typically in powers of two.

```csharp
[Flags]
public enum BorderSides { None=0, Left=1, Right=2, Top=4, Bottom=8 }
```

To work with combined enum values, you use bitwise operators, such as | and &.

```csharp
BorderSides leftRight = BorderSides.Left | BorderSides.Right;

if ((leftRight & BorderSides.Left) != 0)
 Console.WriteLine ("Includes Left"); // Includes Left

string formatted = leftRight.ToString(); // "Left, Right"
BorderSides s = BorderSides.Left;
s |= BorderSides.Right;
Console.WriteLine (s == leftRight); // True

s ^= BorderSides.Right; // Toggles BorderSides.Right
Console.WriteLine (s); // Left
```

By convention, the Flags attribute should always be applied to an enum type when its members are combinable. If you declare such an enum without the Flags attribute, you can still combine members, but calling ToString on an enum instance will emit a number rather than a series of names.

By convention, a combinable enum type is given a plural rather than singular name.
For convenience, you can include combination members within an enum declaration itself

```csharp
[Flags]
public enum BorderSides
{
 None=0,
 Left=1, Right=2, Top=4, Bottom=8,
 LeftRight = Left | Right,
 TopBottom = Top | Bottom,
 All = LeftRight | TopBottom
}
```

# Nested Types

A nested type is declared within the scope of another type.

```csharp
public class TopLevel
{
 public class Nested { } // Nested class
 public enum Color { Red, Blue, Tan } // Nested enum
}
```

A nested type has the following features

- It can access the enclosing type’s private members and everything else the enclosing type can access
- It can be declared with the full range of access modifiers, rather than just public and internal.
- The default accessibility for a nested type is private rather than internal.
- Accessing a nested type from outside the enclosing type requires qualification with the enclosing type’s name (like when accessing static members).

For example, to access `Color.Red` from outside our `TopLevel` class

```csharp
TopLevel.Color color = TopLevel.Color.Red;
```

All types (classes, structs, interfaces, delegates, and enums) can be nested inside
either a class or a struct.

<aside>
💡 If the sole reason for using a nested type is to avoid cluttering a namespace with too many types, consider using a nested namespace instead. A nested type should be used because of its stronger access control restrictions, or when the nested class must access private members of the containing class.

</aside>

# Generics

C# has two separate mechanisms for writing code that is reusable across different types: inheritance and generics. Whereas inheritance expresses reusability with a base type, generics express reusability with a template that contains placeholder types.

### Generic Types

A generic type declares type parameters—placeholder types to be filled in by the consumer of the generic type, which supplies the type arguments. Here is a generic type Stack , designed to stack instances of type T. Stack declares a single type parameter T.

```csharp
public class Stack<T>
{
 int position;
 T[] data = new T[100];
 public void Push (T obj) => data[position++] = obj;
 public T Pop() => data[--position];
}

var stack = new Stack<int>();
stack.Push (5);
stack.Push (10);
int x = stack.Pop(); // x is 10
int y = stack.Pop(); // y is 5
```

Stack <int>fills in the type parameter T with the type argument int, implicitly creating a type on the fly (the synthesis occurs at runtime). Attempting to push a string onto our Stack<int> would, however, produce a compile-time error. Stack<int> effectively has the following definition (substitutions appear in bold, with the class name hashed out to avoid confusion):

```csharp
public class ###
{
 int position;
 int[] data = new int[100];
 public void Push (int obj) => data[position++] = obj;
 public int Pop() => data[--position];
}
```

Technically, we say that Stack<T> is an open type, whereas Stack<int> is a closed type. At runtime, all generic type instances are closed with the placeholder typesfilled in. This means that the following statement is illegal.

```csharp
var stack = new Stack<T>(); // Illegal: What is T?
```

### Why Generics Exist

Generics exist to write code that is reusable across different types. Suppose that we needed a stack of integers, but we didn’t have generic types. One solution would be to hardcode a separate version of the class for every required element type (e.g., IntStack, StringStack, etc.). Clearly, this would cause considerable code duplication. Another solution would be to write a stack that is generalized by using object as the element type:

```csharp
public class ObjectStack
{
 int position;
 object[] data = new object[10];
 public void Push (object obj) => data[position++] = obj;
 public object Pop() => data[--position];
}
```

An ObjectStack, however, wouldn’t work as well as a hardcoded IntStack for specifically stacking integers. An ObjectStack would require boxing and downcasting that could not be checked at compile time.

```csharp
// Suppose we just want to store integers here:
ObjectStack stack = new ObjectStack();
stack.Push ("s"); // Wrong type, but no error!
int i = (int)stack.Pop(); // Downcast - runtime error
```

What we need is both a general implementation of a stack that works for all element types as well as a way to easily specialize that stack to a specific element type for increased type safety and reduced casting and boxing. Generics give us precisely this by allowing us to parameterize the element type.

**Stack<T> has the benefits of both ObjectStack and IntStack. Like ObjectStack, Stack<T> is written once to work generally across all types. Like IntStack, Stack<T> is specialized for a particular type the beauty is that this type is T, which we substitute on the fly.**

### Generic Methods

A generic method declares type parameters within the signature of a method.

With generic methods, many fundamental algorithms can be implemented in a general-purpose way. Here is a generic method that swaps the contents of two variables of any type T.

```csharp
static void Swap<T> (ref T a, ref T b)
{
        T temp = a;
         a = b;
         b = temp;
}
//Swap<T> is called as follows:
int x = 5;
int y = 10;
Swap (ref x, ref y);
```

Generally, there is no need to supply type arguments to a generic method, because
the compiler can implicitly infer the type. If there is ambiguity, generic methods can
be called with type arguments as follows

```csharp
Swap<int> (ref x, ref y);
```

### Declaring Type Parameters

Type parameters can be introduced in the declaration of classes, structs, interfaces, delegates , and methods. Other constructs, such as properties, cannot introduce a type parameter, but they can use one. For example, the property Value uses T:

```csharp
public struct Nullable<T>
{
 public T Value { get; }
}
```

Generic type names and method names can be overloaded as long as the number of type parameters is different. For example, the following three type names do not conflict

```csharp
class A {}
class A<T> {}
class A<T1,T2> {}
```

### The default Generic Value

You can use the `default` keyword to get the default value for a generic type parameter. The default value for a reference type is `null`, and the default value for a value type is the result of bitwise-zeroing the value type’s fields:

```csharp
static void Zap<T> (T[] array)
{
 for (int i = 0; i < array.Length; i++)
 array[i] = default(T);
}
```

### Generic Constraints

By default, you can substitute a type parameter with any type whatsoever. Constraints can be applied to a type parameter to require more specific type arguments.
These are the possible constraints:

```csharp
where T : base-class // Base-class constraint
where T : interface // Interface constraint
where T : class // Reference-type constraint
where T : class? // (See "Nullable reference types")
where T : struct // Value-type constraint (excludes Nullable types)
where T : unmanaged // Unmanaged constraint
where T : new() // Parameterless constructor constraint
where U : T // Naked type constraint
where T : notnull // Non-nullable value type, or from C# 8
 // a non-nullable reference type.
```

```csharp
class SomeClass {}
interface Interface1 {}
class GenericClass<T,U> where T : SomeClass, Interface1
 where U : new()
{...}
```

### Subclassing Generic Types

A generic class can be subclassed just like a nongeneric class. The subclass can leave
the base class’s type parameters open, as in the following example:

```csharp
class Stack<T> {...}
class SpecialStack<T> : Stack<T> {...}
```

the subclass can close the generic type parameters with a concrete type:

```csharp
class IntStack : Stack<int> {...}
```

A subtype can also introduce fresh type arguments

```csharp
class List<T> {...}
class KeyedList<T,TKey> : List<T> {...}
```

### Static Data

Static data is unique for each closed type

```csharp
class Bob<T> { public static int Count; }
class Test
{
 static void Main()
 {
 Console.WriteLine (++Bob<int>.Count); // 1
 Console.WriteLine (++Bob<int>.Count); // 2
 Console.WriteLine (++Bob<string>.Count); // 1
 Console.WriteLine (++Bob<object>.Count); // 1
 }
}
```# Chapter 4 : Advanced C#

ce conversion will succeed:
if (obj is string) Console.WriteLine (((string)obj).Length);

## Delegates

### Overview

A delegate is an object that knows how to call a method.

<aside>
📖 Delegate mean entrust (a task or responsibility) to another person or a person sent or authorized to represent others

</aside>

A delegate type defines the kind of method that delegate instances can call. Specifically, it defines the method’s return type and its parameter types. The following defines a delegate type called **Transformer**:

```csharp
delegate int Transformer (int x);
```

Transformer is compatible with any method with an int return type and a single int parameter, such as this:

```csharp
static int Square (int x) { return x * x; }
```

Assigning a method to a delegate variable creates a delegate instance

```csharp
Transformer t = Square;
```

which can be invoked in the same way as a method:

```csharp
int answer = t(3); // answer is 9
```

### **Example**

```csharp
delegate int Transformer (int x);
class Test
{
 static void Main()
  {
 Transformer t = Square; // Create delegate instance
 int result = t(3); // Invoke delegate
 Console.WriteLine (result); // 9
 }
 static int Square (int x) => x * x;
}
```

A delegate instance literally acts as a delegate for the caller: the caller invokes the delegate, and then the delegate calls the target method. This indirection decouples the caller from the target method.

The statement`Transformer t = Square;`is shorthand for:`Transformer t = new Transformer (Square);`

> Technically, we are specifying a method group when we refer to Square without brackets or arguments. If the method is overloaded, C# will pick the correct overload based on the signature of the delegate to which it’s being assigned.
> 

The expression: `t(3)` is shorthand for: `t.Invoke(3)`

### Writing Plug-in Methods with Delegates

A delegate variable is assigned a method at runtime. This is useful for writing plugin methods. In this example, we have a utility method named Transform that applies a transform to each element in an integer array. The Transform method has a delegate parameter, for specifying a plug-in transform.

```csharp
public delegate int Transformer (int x);
class Util
{
 public static void Transform (int[] values, Transformer t)
 {
 for (int i = 0; i < values.Length; i++)
 values[i] = t (values[i]);
 }
}
class Test
{
 static void Main()
 {
 int[] values = { 1, 2, 3 };
 Util.Transform (values, Square); // Hook in the Square method
 foreach (int i in values)
 Console.Write (i + " "); // 1 4 9
 }
 static int Square (int x) => x * x;
}
```

Our Transform method is a higher-order function, because it’s a function that takes a function as an argument. (A method that returns a delegate would also be a higher-order function.)

### Multicast Delegates

All delegate instances have multicast capability. This means that a delegate instance can reference not just a single target method, but also a list of target methods. The + and += operators combine delegate instances. For example:

```csharp
SomeDelegate d = SomeMethod1;
d += SomeMethod2;
```

The last line is functionally the same as:

```csharp
d = d + SomeMethod2;
```

Invoking d will now call both SomeMethod1 and SomeMethod2. Delegates are invoked in the order they are added.

The - and -= operators remove the right delegate operand from the left delegate operand. For example:

```csharp
d -= SomeMethod1;
```

Invoking d will now cause only SomeMethod2 to be invoked.

### Multicast delegate example

Suppose you wrote a method that took a long time to execute. That method could regularly report progress to its caller by invoking a delegate. In this example, the HardWork method has a ProgressReporter delegate parameter, which it invokes to indicate progress:

```csharp
public delegate void ProgressReporter (int percentComplete);
public class Util
{
 public static void HardWork (ProgressReporter p)
 {
 for (int i = 0; i < 10; i++)
 {
 p (i * 10); // Invoke delegate
 System.Threading.Thread.Sleep (100); // Simulate hard work
 }
 }
}
class Test
{
 static void Main()
 {
 ProgressReporter p = WriteProgressToConsole;
 p += WriteProgressToFile;
 Util.HardWork (p);
 }
 static void WriteProgressToConsole (int percentComplete)
 => Console.WriteLine (percentComplete);
 static void WriteProgressToFile (int percentComplete)
 => System.IO.File.WriteAllText ("progress.txt",
 percentComplete.ToString());
}
```

### Generic Delegate Types

A delegate type can contain generic type parameters

```csharp
public delegate T Transformer<T> (T arg);
public class Util
{
 public static void Transform<T> (T[] values, Transformer<T> t)
 {
 for (int i = 0; i < values.Length; i++)
 values[i] = t (values[i]);
 }
}
class Test
{
 static void Main()
 {
 int[] values = { 1, 2, 3 };
 Util.Transform (values, Square); // Hook in Square
 foreach (int i in values)
 Console.Write (i + " "); // 1 4 9
 }
 static int Square (int x) => x * x;
}
```

### The Func and Action Delegates

With generic delegates, it becomes possible to write a small set of delegate types that
are so general they can work for methods of any return type and any (reasonable)
number of arguments. These delegates are the Func and Action delegates, defined in
the System namespace (the in and out annotations indicate variance, which we
cover in the context of delegates shortly):

```csharp
delegate TResult Func <out TResult> ();
delegate TResult Func <in T, out TResult> (T arg);
delegate TResult Func <in T1, in T2, out TResult> (T1 arg1, T2 arg2);
... and so on, up to T16
delegate void Action ();
delegate void Action <in T> (T arg);
delegate void Action <in T1, in T2> (T1 arg1, T2 arg2);
... and so on, up to T16

public static void Transform<T> (T[] values, Func<T,T> transformer)
{
 for (int i = 0; i < values.Length; i++)
 values[i] = transformer (values[i]);
}
```

### Delegates Versus Interfaces

A problem that can be solved with a delegate can also be solved with an interface. For instance, we can rewrite our original example with an interface called ITrans former instead of a delegate:

```csharp
public interface ITransformer
{
 int Transform (int x);
}
public class Util
{
 public static void TransformAll (int[] values, ITransformer t)
 {
 for (int i = 0; i < values.Length; i++)
 values[i] = t.Transform (values[i]);
 }
 }
class Squarer : ITransformer
{
 public int Transform (int x) => x * x;
}
...
static void Main()
{
 int[] values = { 1, 2, 3 };
 Util.TransformAll (values, new Squarer());
 foreach (int i in values)
 Console.WriteLine (i);
}   
```

A delegate design may be a better choice than an interface design if one or more of these conditions are true:

- The interface defines only a single method.
- Multicast capability is needed.
- The subscriber needs to implement the interface multiple times

*In the ITransformer example, we don’t need to multicast. However, the interface defines only a single method. Furthermore, our subscriber may need to implement ITransformer multiple times, to support different transforms, such as square or cube. With interfaces, we’re forced into writing a separate type per transform, since Test can implement ITransformer only once. This is quite cumbersome:*

```csharp
class Squarer : ITransformer
{
 public int Transform (int x) => x * x;
}
class Cuber : ITransformer
{
 public int Transform (int x) => x * x * x;
}
...
static void Main()
{
 int[] values = { 1, 2, 3 };
 Util.TransformAll (values, new Cuber());
 foreach (int i in values)
 Console.WriteLine (i);
}
```

### Delegate Compatibility

Delegate types are all incompatible with one another, even if their signatures are the
same

```csharp
delegate void D1();
delegate void D2();
...
D1 d1 = Method1;
D2 d2 = d1; // Compile-time error

//Delegate instances are considered equal if they have the same method targets
delegate void D();
...
D d1 = Method1;
D d2 = Method1;
Console.WriteLine (d1 == d2); // True

```

Multicast delegates are considered equal if they reference the same methods in the same order. 

## Events

### Overview

When using delegates, two emergent roles commonly appear: broadcaster and sub‐ scriber. The broadcaster is a type that contains a delegate field. The broadcaster decides when to broadcast, by invoking the delegate. The subscribers are the method target recipients. A subscriber decides when to start and stop listening, by calling += and -= on the broadcaster’s delegate. A subscriber does not know about, or interfere with, other subscribers.

Events are a language feature that formalizes this pattern. An event is a construct that exposes just the subset of delegate features required for the broadcaster/ subscriber model. The main purpose of events is to prevent subscribers from interfering with one another.

The easiest way to declare an event is to put the event keyword in front of a dele‐ gate member:

```csharp
// Delegate definition
public delegate void PriceChangedHandler (decimal oldPrice,
 decimal newPrice);
public class Broadcaster
{
 // Event declaration
 public event PriceChangedHandler PriceChanged;
}
```

Code within the Broadcaster type has full access to PriceChanged and can treat it as a delegate. Code outside of Broadcaster can only perform += and -= operations on the PriceChanged event.

Consider the following example. The Stock class fires its PriceChanged event every time the Price of the Stock changes:

```csharp
public delegate void PriceChangedHandler (decimal oldPrice,
 decimal newPrice);
public class Stock
{
 string symbol;
 decimal price;
 public Stock (string symbol) { this.symbol = symbol; }
 public event PriceChangedHandler PriceChanged;
 public decimal Price
 {
 get { return price; }
 set
 {
 if (price == value) return; // Exit if nothing has changed
 decimal oldPrice = price;
 price = value;
  if (PriceChanged != null) // If invocation list not
 PriceChanged (oldPrice, price); // empty, fire event.
 }
 }
}
```

If we remove the event keyword from our example so that PriceChanged becomes an ordinary delegate field, our example would give the same results. However, Stock would be less robust, in that subscribers could do the following things to interfere with each other:

- Replace other subscribers by reassigning PriceChanged (instead of using the += operator).
- Clear all subscribers (by setting PriceChanged to null).
- Broadcast to other subscribers by invoking the delegate

## Lambda Expressions

A lambda expression is an unnamed method written in place of a delegate instance. The compiler immediately converts the lambda expression to either:

- A delegate instance.
- An expression tree, of type Expression, representing the code inside the lambda expression in a traversable object model. This allows the lambda expression to be interpreted later at runtime

Given the following delegate type: `delegate int Transformer (int i);`

e could assign and invoke the lambda expression x => x * x as follows:

```csharp
Transformer sqr = x => x * x;
Console.WriteLine (sqr(3)); // 9
```

> Internally, the compiler resolves lambda expressions of this type by writing a private method, and moving the expression’s code into that method.
> 

A lambda expression has the following form:`(parameters) => expression-or-statement-block`

```csharp
x=>x*x;
(x)=>x*x;
(int x)=>x*x;
(int x,int y)=>x*y;
x => { return x * x; };
```

```csharp
// A lambda expression is an unnamed method written in place of a delegate instance.
// A lambda expression has the following form:
//   (parameters) => expression-or-statement-block

delegate int Transformer (int i);

static void Main()
{
    Transformer sqr = x => x * x;
    Console.WriteLine (sqr(3));    // 9
    
    // Using a statement block instead:
    Transformer sqrBlock = x => { return x * x; };
    Console.WriteLine (sqr(3));
    
    // Using a generic System.Func delegate:
    Func<int,int> sqrFunc = x => x * x;
    Console.WriteLine (sqrFunc(3));
    
    // Using multiple arguments:
    Func<string,string,int> totalLength = (s1, s2) => s1.Length + s2.Length;
    int total = totalLength ("hello", "world");
    total.Dump ("total");
    
    // Explicitly specifying parameter types:
    Func<int,int> sqrExplicit = (int x) => x * x;
    Console.WriteLine (sqrFunc(3));
}
```

### Explicitly Specifying Lambda Parameter Types

The compiler can usually infer the type of lambda parameters contextually. When this is not the case, you must specify the type of each parameter explicitly. Consider the following two methods:

```csharp
void Foo<T> (T x) {}
void Bar<T> (Action<T> a) {}
//The following code will fail to compile, because the compiler cannot infer the type
//of x
****Bar (x => Foo (x));
//We can fix this by explicitly specifying x’s type as follows
Bar (x => Foo (x));
```

### Capturing Outer Variables

A lambda expression can reference the local variables and parameters of the method
in which it’s defined

```csharp
// A lambda expression can reference the local variables and parameters of the method
// in which it’s defined (outer variables)

int factor = 2;
Func<int, int> multiplier = n => n * factor;
Console.WriteLine (multiplier (3));           // 6

// Captured variables are evaluated when the delegate is invoked, not when the variables were captured:

factor = 10;
Console.WriteLine (multiplier (3));           // 30

// Lambda expressions can themselves update captured variables:

int seed = 0;
Func<int> natural = () => seed++;
Console.WriteLine (natural());           // 0
Console.WriteLine (natural());           // 1
Console.WriteLine (seed);                // 2
```

### Lambda Expressions Versus Local Methods

The functionality of local methods overlaps with that of lambda expressions.

Local methods have the following three advantages

- They can be recursive (they can call themselves), without ugly hacks
- They avoid the clutter of specifying a delegate type
- They incur slightly less overhead

Local methods are more efficient because they avoid the indirection of a delegate (which costs some CPU cycles and a memory allocation). They can also access local variables of the containing method without the compiler having to “hoist” the captured variables into a hidden class.

# Try Statements and Exceptions

A try statement specifies a code block subject to error-handling or cleanup code. The try block must be followed by one or more catch blocks, a finally block, or both. The catch block executes when an error is thrown in the try block. The finally block executes after execution leaves the try block (or if present, the catch block), to perform cleanup code, regardless of whether an exception was thrown.

A catch block has access to an Exception object that contains information about the error. You use a catch block to either compensate for the error or rethrow the exception. You rethrow an exception if you merely want to log the problem or if you want to rethrow a new, higher-level exception type.

```csharp
try
{
 ... // exception may get thrown within execution of this block
}
catch (ExceptionA ex)
{
 ... // handle exception of type ExceptionA
}
catch (ExceptionB ex)
{
 ... // handle exception of type ExceptionB
}
finally
{
 ... // cleanup code
}
```

```csharp
// We can catch the DivideByZeroException as follows:

static int Calc (int x) { return 10 / x; }

static void Main()
{
    try
    {
        int y = Calc (0);
        Console.WriteLine (y);
    }
    catch (DivideByZeroException ex)
    {
        Console.WriteLine ("x cannot be zero");
    }
    Console.WriteLine ("program completed");
}
```

When an exception is thrown within a try statement, the CLR performs a test:

Does the try statement have any compatible catch blocks?

- If so, execution jumps to the compatible catch block, followed by the finally block (if present), and then execution continues normally.
- If not, execution jumps directly to the finally block (if present), then the CLR looks up the call stack for other try blocks; if found, it repeats the test.

### Exception filters

You can specify an exception filter in a catch clause by adding a when clause

```csharp
catch (WebException ex) when (ex.Status == WebExceptionStatus.Timeout)
{
 ...
}
```

If a WebException is thrown in this example, the Boolean expression following the when keyword is then evaluated. If the result is false, the catch block in question is ignored and any subsequent catch clauses are considered. 

### The using statement

The using statement provides an elegant syntax for calling Dispose on  an IDisposable object within a finally block:

```csharp
static void ReadFile()
{
    using (StreamReader reader = File.OpenText ("file.txt"))
    {
        if (reader.EndOfStream) return;
        Console.WriteLine (reader.ReadToEnd());
    }
}

static void Main()
{
    File.WriteAllText ("file.txt", "test");
    ReadFile ();
}
```

### Using declarations

If you omit the brackets and statement block following a using statement, it
becomes a using declaration. The resource is then disposed when execution falls out‐
side the enclosing statement block

```csharp
if (File.Exists ("file.txt"))
{
    using var reader = File.OpenText ("file.txt");
    Console.WriteLine (reader.ReadLine());
}
```

### Throwing Exceptions

- Exceptions can be thrown either by the runtime or in user code:

```csharp
static void Display (string name)
{
    if (name == null)
        throw new ArgumentNullException (nameof (name));

    Console.WriteLine (name);
}

static void Main()
{
    try
    {
        Display (null);
    }
    catch (ArgumentNullException ex)
    {
        Console.WriteLine ("Caught the exception");
    }
}
```

### Throw expressions

```csharp
// Prior to C# 7, throw was always a statement. Now it can also appear as an expression in
// expression-bodied functions:

public string Foo() => throw new NotImplementedException();

// A throw expression can also appear in a ternary conditional expression:

string ProperCase (string value) =>
    value == null ? throw new ArgumentException ("value") :
    value == "" ? "" :
    char.ToUpper (value [0]) + value.Substring (1);

void Main()
{
    ProperCase ("test").Dump();
    ProperCase (null).Dump();     // throws an ArgumentException
}
```

### Rethrowing an exception

```csharp
// Rethrowing lets you back out of handling an exception should circumstances turn out to be
// outside what you expected:

string s = null;

using (WebClient wc = new WebClient())
    try { s = wc.DownloadString ("http://www.albahari.com/nutshell/");  }
    catch (WebException ex)
    {
        if (ex.Status == WebExceptionStatus.NameResolutionFailure)
            Console.WriteLine ("Bad domain name");
        else
            throw;     // Can’t handle other sorts of WebException, so rethrow
    }

s.Dump();
```

### Key Properties of System.Exception

StackTrace

- A string representing all the methods that are called from the origin of the exception to the catch block.

Message

- A string with a description of the error.

### Common Exception Types

**System.ArgumentException**
Thrown when a function is called with a bogus argument. This generally indicates a program bug

**System.ArgumentNullException**
Subclass of ArgumentException that’s thrown when a function argument is (unexpectedly) null.

**System.ArgumentOutOfRangeException**
Subclass of ArgumentException that’s thrown when a (usually numeric) argument is too big or too small. For example, this is thrown when passing a negative number into a function that accepts only positive values.

**System.InvalidOperationException**
Thrown when the state of an object is unsuitable for a method to successfully execute, regardless of any particular argument values. Examples include reading an unopened file or getting the next element from an enumerator for which the underlying list has been modified partway through the iteration.

**System.NotSupportedException**
Thrown to indicate that a particular functionality is not supported. A good example is calling the Add method on a collection for which IsReadOnly returns true.

**System.NotImplementedException**
Thrown to indicate that a function has not yet been implemented.

**System.ObjectDisposedException**
Thrown when the object upon which the function is called has been disposed.

# Enumeration and Iterators

### Enumeration

An enumerator is a read-only, forward-only cursor over a sequence of values. C# treats a type as an enumerator if it does any of the following

- Implements `System.Collections.IEnumerator`
- Implements `System.Collections.Generic.IEnumerator`
- Has a public parameterless method named `MoveNext` and property called
`Current`

The foreach statement iterates over an enumerable object. An enumerable object is the logical representation of a sequence.

```csharp
// High-level way of iterating through the characters in the word “beer”:

foreach (char c in "beer")
    Console.WriteLine (c);

// Low-level way of iterating through the same characters:

using (var enumerator = "beer".GetEnumerator())
    while (enumerator.MoveNext())
    {
        var element = enumerator.Current;
        Console.WriteLine (element);
    }
```

### **Collection Initializers**

```csharp
// You can instantiate and populate an enumerable object in a single step with collection initializers:
{
    List<int> list = new List<int> {1, 2, 3};	
    list.Dump();
}

// Equivalent to:
{
    List<int> list = new List<int>();
    list.Add (1);
    list.Add (2);
    list.Add (3);	
    list.Dump();
}
```

### Iterators

Whereas a foreach statement is a consumer of an enumerator, an iterator is a producer of an enumerator

```csharp
static void Main()
{
    foreach (int fib in Fibs(6))
        Console.Write (fib + "  ");
}

static IEnumerable<int> Fibs (int fibCount)
{
    for (int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++)
    {
        yield return prevFib;
        int newFib = prevFib+curFib;
        prevFib = curFib;
        curFib = newFib;
    }
}
//1  1  2  3  5  8
```

Whereas a return statement expresses **“Here’s the value you asked me to return from this method,”** a yield return statement expresses **“Here’s the next element you asked me to yield from this enumerator.”** On each yield statement, control is returned to the caller, but the callee’s state is maintained so that the method can continue executing as soon as the caller enumerates the next element. The lifetime of this state is bound to the enumerator such that the state can be released when the caller has finished enumerating.

### Iterator Semantics

An iterator is a method, property, or indexer that contains one or more yield statements. An iterator must return one of the following four interfaces (otherwise, the compiler will generate an error):

```csharp
// Enumerable interfaces
System.Collections.IEnumerable
System.Collections.Generic.IEnumerable<T>
// Enumerator interfaces
System.Collections.IEnumerator
System.Collections.Generic.IEnumerator<T>
```

Multiple yield statements are permitted

```csharp
class Test
{
 static void Main()
 {
 foreach (string s in Foo())
 Console.WriteLine(s); // Prints "One","Two","Three"
 }
 static IEnumerable<string> Foo()
 {
 yield return "One";
 yield return "Two";
 yield return "Three";
 }
}
```

### yield break

A return statement is illegal in an iterator block; instead you must use the yield break statement to indicate that the iterator block should exit early, without returning more elements.

```csharp
// The yield break statement indicates that the iterator block should exit early,
// without returning more elements:

static void Main()
{
    foreach (string s in Foo (true))
        Console.WriteLine(s);
}

static IEnumerable<string> Foo (bool breakEarly)
{
    yield return "One";
    yield return "Two";
    
    if (breakEarly)
        yield break;
    
    yield return "Three";
}
```

### Iterators and try/catch/finally blocks

```csharp
// A yield return statement cannot appear in a try block that has a catch clause:

static void Main()
{
    foreach (string s in Foo())
        Console.WriteLine(s);
}

static IEnumerable<string> Foo()
{
    try { yield return "One"; }		// Illegal
    catch { /*...*/ }
}
```

```csharp
// You can, however, yield within a try block that has (only) a finally block:

static void Main()
{
    foreach (string s in Foo()) s.Dump();	
    Console.WriteLine();		
    foreach (string s in Foo())
    {
        ("First element is " + s).Dump();
        break;
    }
}

static IEnumerable<string> Foo()
{
    try 
    {
        yield return "One";
        yield return "Two";
        yield return "Three";
    }
    finally { "Finally".Dump(); }
}
```

### Composing Sequences

```csharp
// Iterators are highly composable:

static void Main()
{
    foreach (int fib in EvenNumbersOnly (Fibs(6)))
        Console.WriteLine (fib);
}

static IEnumerable<int> Fibs (int fibCount)
{
    for (int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++)
    {
        yield return prevFib;
        int newFib = prevFib+curFib;
        prevFib = curFib;
        curFib = newFib;
    }
}

static IEnumerable<int> EvenNumbersOnly (IEnumerable<int> sequence)
{
    foreach (int x in sequence)
        if ((x % 2) == 0)
            yield return x;
}
```

# Nullable Types

Reference types can represent a nonexistent value with a null reference. Value types, however, cannot ordinarily represent null values. For example:

```csharp
string s = null; // OK, Reference Type
int i = null; // Compile Error, Value Type cannot be null
```

To represent null in a value type, you must use a special construct called a nullable type. A nullable type is denoted with a value type followed by the ? symbol:

```csharp
int? i = null; // OK, Nullable Type
Console.WriteLine (i == null); // True
```

### Nullable<T> Struct

T? translates into System.Nullable, which is a lightweight immutable structure, having only two fields, to represent Value and HasValue. The essence of System .Nullable is very simple:

```csharp
public struct Nullable<T> where T : struct
{
 public T Value {get;}
 public bool HasValue {get;}
  public T GetValueOrDefault();
 public T GetValueOrDefault (T defaultValue);
 ...
}
//The code:
int? i = null;
Console.WriteLine (i == null); // True
//translates to:
Nullable<int> i = new Nullable<int>();
Console.WriteLine (! i.HasValue); // True
```

### Scenarios for Nullable Value Types

One of the most common scenarios for nullable value types is to represent unknown values. This frequently occurs in database programming, where a class is mapped to a table with nullable columns. If these columns are strings (e.g., an EmailAddress column on a Customer table), there is no problem because string is a reference type in the CLR, which can be null. 

# Extension Methods

Extension methods allow an existing type to be extended with new methods without altering the definition of the original type. An extension method is a static method of a static class, where the this modifier is applied to the first parameter. The type of the first parameter will be the type that is extended. For example:

```csharp
public static class StringHelper
{
 public static bool IsCapitalized (this string s)
 {
 if (string.IsNullOrEmpty(s)) return false;
 return char.IsUpper (s[0]);
 }
}
```

The IsCapitalized extension method can be called as though it were an instance method on a string, as follows:

```csharp
Console.WriteLine ("Perth".IsCapitalized());
```

An extension method call, when compiled, is translated back into an ordinary static method call:

```csharp
Console.WriteLine (StringHelper.IsCapitalized ("Perth"));
```

### Extension Method Chaining

Extension methods, like instance methods, provide a tidy way to chain functions. Consider the following two functions:

```csharp
public static class StringHelper
{
 public static string Pluralize (this string s) {...}
 public static string Capitalize (this string s) {...}
}
//x and y are equivalent and both evaluate to "Sausages", but x uses extension meth‐
//ods, whereas y uses static methods:
string x = "sausage".Pluralize().Capitalize();
string y = StringHelper.Capitalize (StringHelper.Pluralize ("sausage"));
```

### Extension Method Namespaces

An extension method cannot be accessed unless its class is in scope, typically by its namespace being imported.

```csharp
using System;
namespace Utils
{
 public static class StringHelper
 {
 public static bool IsCapitalized (this string s)
 {
 if (string.IsNullOrEmpty(s)) return false;
 return char.IsUpper (s[0]);
 }
 }
}

namespace MyApp
{
 using Utils;
 class Test
 {
static void Main() => Console.WriteLine ("Perth".IsCapitalized());
 }
}
```

# Anonymous Types

An anonymous type is a simple class created by the compiler on the fly to store a set
of values. To create an anonymous type, use the `new` keyword followed by an object
initializer, specifying the properties and values the type will contain.

```csharp
//You must use the var keyword to reference an anonymous type because it doesn’t
//have a name
var dude = new { Name = "Bob", Age = 23 };

//The compiler translates this to (approximately) the following

internal class AnonymousGeneratedTypeName
{
 private string name; // Actual field name is irrelevant
 private int age; // Actual field name is irrelevant
 public AnonymousGeneratedTypeName (string name, int age)
 {
 this.name = name; this.age = age;
 }
 public string Name { get { return name; } }
 public int Age { get { return age; } }
 // The Equals and GetHashCode methods are overridden (see Chapter 6).
 // The ToString method is also overridden.
}
...
var dude = new AnonymousGeneratedTypeName ("Bob", 23);
```

## Tuples

**Like anonymous types, tuples provide a simple way to store a set of values. The main purpose of tuples is to safely return multiple values from a method without resorting to out parameters**

The simplest way to create a tuple literal is to list the desired values in parentheses. This creates a tuple with unnamed elements, which you refer to as Item1, Item2, and so on:

```csharp
var bob = ("Bob", 23); // Allow compiler to infer the element types
Console.WriteLine (bob.Item1); // Bob
Console.WriteLine (bob.Item2); // 23

//Tuples are value types, with mutable (read/write) elements:
var joe = bob; // joe is a *copy* of job
joe.Item1 = "Joe"; // Change joe's Item1 from Bob to Joe
Console.WriteLine (bob); // (Bob, 23)
Console.WriteLine (joe); // (Joe, 23)

//Unlike with anonymous types, you can specify a tuple type explicitly. Just list each
//of the element types in parentheses:
(string,int) bob = ("Bob", 23); // var is not compulsory with tuples!

//This means that you can usefully return a tuple from a method:
static (string,int) GetPerson() => ("Bob", 23);
static void Main()
{
 (string,int) person = GetPerson(); // Could use 'var' here if we want
 Console.WriteLine (person.Item1); // Bob
 Console.WriteLine (person.Item2); // 23
}

```

### Naming Tuple Elements

You can optionally give meaningful names to elements when creating tuple literals:

```csharp
var tuple = (Name:"Bob", Age:23);
Console.WriteLine (tuple.Name); // Bob
Console.WriteLine (tuple.Age); // 23
```

You can do the same when specifying tuple types:

```csharp
static (string Name, int Age) GetPerson() => ("Bob", 23);
static void Main()
{
 var person = GetPerson();
 Console.WriteLine (person.Name); // Bob
 Console.WriteLine (person.Age); // 23
}
```

### Deconstructing Tuples

```csharp
var bob = ("Bob", 23);

(string name, int age) = bob;   // Deconstruct the bob tuple into
                                // separate variables (name and age).
Console.WriteLine (name);
Console.WriteLine (age);
```

# Patterns

In Chapter 3, we demonstrated how to use the is operator to test whether a refer‐
ence conversion will succeed:

```csharp
if (obj is string)
 Console.WriteLine (((string)obj).Length);

if (obj is string s)
 Console.WriteLine (s.Length);
```

This employs one kind of pattern called a type pattern.

Patterns are supported in the following contexts

- After the is operator (variable is pattern)
- In switch statements
- In switch expressions

**Most of these patterns are intended for use in switch statements/expressions, where they do the following:**

- **Reduce the need for when clauses**
- **Let you use switches where you couldn’t previously**

### Tuple Patterns

Tuple patterns provide a simple mechanism for switching on multiple values

```csharp
enum Season { Spring, Summer, Fall, Winter };
int AverageCelsiusTemperature (Season season, bool daytime) =>
 (season, daytime) switch
 {
 (Season.Spring, true) => 20,
 (Season.Spring, false) => 16,
 (Season.Summer, true) => 27,
 (Season.Summer, false) => 22,
 (Season.Fall, true) => 18,
 (Season.Fall, false) => 12,
 (Season.Winter, true) => 10,
 (Season.Winter, false) => -2,
 _ => throw new Exception ("Unexpected combination")
};
```

### Positional Patterns

For types that define a Deconstruct method

```csharp
class Point
{
 public readonly int X, Y;
 public Point (int x, int y) => (X, Y) = (x, y);
 public void Deconstruct (out int x, out int y)
 {
 x = X; y = Y;
 }
}
```

you can use the object’s positional properties for pattern matching

```csharp
var p = new Point (2, 3);
Console.WriteLine (p is (2, 3)); // true
```

With a switch:

```csharp
string Print (object obj) => obj switch
{
 Point (0, 0) => "Empty point",
 Point (var x, var y) when x == y => "Diagonal"
 ...
};
```

## Operator Overloading

Operators can be overloaded to provide more natural syntax for custom types. Operator overloading is most appropriately used for implementing custom structs that represent fairly primitive data types. For example, a custom numeric type is an excellent candidate for operator overloading.

The following symbolic operators can be overloaded:

```csharp
+ (unary) - (unary) ! ˜ ++
-- + - * /
% & | ^ <<
>> == != > <
>= <=
```

### Operator Functions

An operator is overloaded by declaring an operator function. An operator function has the following rules:

- The name of the function is specified with the operator keyword followed by an operator symbol
- The operator function must be marked static and public.
- The parameters of the operator function represent the operands.
- The return type of an operator function represents the result of an expression.
- At least one of the operands must be the type in which the operator function is declared.

In the following example, we define a struct called Note representing a musical note, and then overload the + operator:

```csharp
public struct Note
{
 int value;
 public Note (int semitonesFromA) { value = semitonesFromA; }
 public static Note operator + (Note x, int semitones)
 {
 return new Note (x.value + semitones);
 }
}
//This overload allows us to add an int to a Note:
Note B = new Note (2);
Note CSharp = B + 2;
```

### Overloading true and false

```csharp
SqlBoolean a = SqlBoolean.Null;
if (a)
 Console.WriteLine ("True");
else if (!a)
 Console.WriteLine ("False");
else
 Console.WriteLine ("Null");
OUTPUT:
Null

public struct SqlBoolean
{
 public static bool operator true (SqlBoolean x)
 => x.m_value == True.m_value;
 public static bool operator false (SqlBoolean x)
 => x.m_value == False.m_value;
 public static SqlBoolean operator ! (SqlBoolean x)
 {
 if (x.m_value == Null.m_value) return Null;
 if (x.m_value == False.m_value) return True;
 return False;
 }
 public static readonly SqlBoolean Null = new SqlBoolean(0);
 public static readonly SqlBoolean False = new SqlBoolean(1);
 public static readonly SqlBoolean True = new SqlBoolean(2);
 private SqlBoolean (byte value) { m_value = value; }
 private byte m_value;
}
```

## Preprocessor Directives

Preprocessor directives supply the compiler with additional information about regions of code. The most common preprocessor directives are the conditional directives, which provide a way to include or exclude regions of code from compilation. For example:

```csharp
#define DEBUG
class MyClass
{
 int x;
 void Foo()
 {
 #if DEBUG
 Console.WriteLine ("Testing: x = {0}", x);
 #endif
 }
 ...
}
```

In this class, the statement in Foo is compiled as conditionally dependent upon the
presence of the DEBUG symbol. If we remove the DEBUG symbol, the statement is not
compiled. **You can define preprocessor symbols within a source file (as we have
done), or at a project level in the .csproj file:**

```csharp
<PropertyGroup>
 <DefineConstants>DEBUG;ANOTHERSYMBOL</DefineConstants>
</PropertyGroup>
```

Preprocessor directives

- \#define symbol - Defines symbol
- \#if symbol [operator sym bol2]...-symbol to test
- \#else - Executes code to subsequent #endif
- \#endif - Ends conditional directives
- \#warning text - text of the warning to appear in compiler output
- \#error text - text of the error to appear in compiler output

## XML Documentation

A documentation comment is a piece of embedded XML that documents a type or member. A documentation comment comes immediately before a type or member declaration, and starts with three slashes:

```csharp
/// <summary>Cancels a running query.</summary>
public void Cancel() { ... }

//Multiline comments can be done either like this:
/// <summary>
/// Cancels a running query
/// </summary>
public void Cancel() { ... }

//or like this (notice the extra star at the start):
/**
 <summary> Cancels a running query. </summary>
*/
public void Cancel() { ... }
```

If you compile with the /doc directive (in Visual Studio, go to the Build tab of Project Properties), the compiler extracts and collates documentation comments into a single XML file. This has two main uses:

- If placed in the same folder as the compiled assembly, Visual Studio (and LINQPad) automatically read the XML file and use the information to provide IntelliSense member listings to consumers of the assembly of the same name.
- Third-party tools (such as Sandcastle and NDoc) can transform the XML file into an HTML help file.# Chapter 5 : Framework Overview

# What’s New in .NET Core 3

The new core features of .NET Core 3 include a built-in high-performance JSON reader/writer and serializer (see Chapter 11), and support for C# 8 and .NET Standard 2.1.

Note that .NET Core 3 succeeds both .NET Core 2.2 and .NET Framework. If you’re
coming from .NET Framework, the following features are all new

- Built-in immutable collections (see “Immutable Collections” on page 357 in
Chapter 7)
- AssemblyLoadContext: a new API for loading, resolving, and isolating assem‐
blies that significantly improves on Assembly.LoadFile and Assembly.LoadFrom
(see “Loading, Resolving, and Isolating Assemblies” on page 775 in Chapter 18)
- The Span and Memory structs, which help reduce memory allocations in
performance hotspots (see Chapter 24)
- Array and memory pooling to reduce the load on the garbage collector (see
“Array Pooling” on page 541 in Chapter 12)
- A new EventCounter API for performance monitoring (see Chapter 13)
- Startup hooks for injecting code into an application’s Main method at runtime

As well as general performance improvements to the CLR and garbage collec‐
tor, .NET Core 3’s CLR includes an improvement to Just-In-Time (JIT) compilation,
called tiered compilation, whereby the CLR automatically identifies performance
hotspots as a program is running and then selectively re-JITs the Intermediate Lan‐
guage to higher-quality native code.

.NET Core 3 also has new deployment features

- An Ahead-Of-Time (AOT) compilation option, allowing an application to be
compiled to native code before being deployed (in lieu of .NET Framework’s
client-side native image generation tool)
- Support for single-file executables with assembly linking to trim unused
assemblies
- Support for MSIX, a new Windows deployment format

# NET Standard 2.0

A library that targets .NET Standard 2.0 instead of a specific framework is usefully portable. The same assembly will run without modification on most of today’s popular frameworks, including the following

- .NET Core 2.0+
- UWP 10.0.16299+
- Mono 5.4+
- .NET Framework 4.6.1+

To target .NET Standard 2.0, add the following to your .csproj file:

```csharp
<PropertyGroup>
 <TargetFramework>netstandard2.0</TargetFramework>
 <PropertyGroup>
```

<aside>
📢 .NET Standard is not a framework; it’s merely a specification describing a minimum baseline of functionality (types and members) that guarantees compatibility with a certain set of frameworks. The concept is similar to C# interfaces: .NET Standard is like an interface that concrete types (frameworks) can implement.

</aside>

### .NET Framework and .NET Core Compatibility

Because .NET Framework has existed for so long, it’s not uncommon to encounter libraries that are available only for .NET Framework (with no .NET Standard or .NET Core equivalent). To help mitigate this situation, .NET Core projects are permitted to reference .NET Framework assemblies, with the following provisos

- An exception is thrown should the .NET Framework assembly call an API that’s not supported in .NET Core.
- Nontrivial dependencies might fail to resolve.

In practice, it’s most likely to work with assemblies that perform a simple function, such as wrapping an unmanaged DLL, or that rely on a single well-supported API, such as WPF or Windows Forms.

### Framework and C# Language Versions

The C# compiler chooses a language version automatically based on the framework
that your project targets

- For .NET Core 3.x and .NET Standard 2.1, it chooses C# 8.
- For .NET Core 2.x, .NET Framework, and .NET Standard 2.0 and below, it
chooses C# 7.3.

# Reference Assemblies

When you target .NET Standard, your project implicitly references an assembly called netstandard.dll, which contains all of the allowable types and members for your chosen version of .NET Standard. This is called a reference assembly because it exists only for the benefit of the compiler and contains no compiled code. At runtime, the “real” assemblies are identified through assembly redirection attributes (the choice of assemblies will depend on which framework and platform the assembly eventually runs on).

Interestingly, a similar thing happens when you target .NET Core. Your project implicitly references a set of reference assemblies whose types mirror what’s in the runtime assemblies for the chosen .NET Core version. This helps with versioning and cross-platform compatibility, and also allows you to target a different .NET Core version than what is installed on your machine. For instance, if you’ve
installed .NET Core 3, your project can still target .NET Core 2.2, and thanks to a set of reference assemblies, the compiler will see only the types and members available to .NET Core 2.2.

# The CLR and BCL

### System Types

The most fundamental types live directly in the System namespace. These include C#’s built-in types, the Exception base class, the Enum, Array, and Delegate base classes, and Nullable, Type, DateTime, TimeSpan, and Guid.

The System namespace also includes types for performing mathematical functions (Math), generating random numbers (Random), and converting between various types (Convert and
BitConverter).

The System namespace also defines the IDisposable interface and the GC class for interacting with the garbage collector

### Text Processing

The System.Text namespace contains the StringBuilder class (the editable or mutable cousin of string) and the types for working with text encodings, such as UTF-8 (Encoding and its subtypes).

The System.Text.RegularExpressions namespace contains types that perform advanced pattern-based search-and-replace operations

### Collections

.NET Core offers a variety of classes for managing collections of items. These include both list- and dictionary-based structures; they work in conjunction with a set of standard interfaces that unify their common characteristics.

```csharp
System.Collections // Nongeneric collections
System.Collections.Generic // Generic collections
System.Collections.Specialized // Strongly typed collections
System.Collections.ObjectModel // Bases for your own collections
System.Collections.Concurrent // Thread-safe collection (Chapter 23)
```

### Querying

LINQ allows you to perform type-safe queries over local and remote collections. A big advantage
of LINQ is that it presents a consistent querying API across a variety of domains.

```csharp
System.Linq // LINQ to Objects and PLINQ
System.Linq.Expressions // For building expressions manually
System.Xml.Linq // LINQ to XML
```

### XML and JSON

XML and JSON are widely supported in .NET Core.

```csharp
System.Xml // XmlReader, XmlWriter
System.Xml.Linq // The LINQ to XML DOM
System.Xml.Schema // Support for XSD
System.Xml.Serialization // Declarative XML serialization for .NET types
System.Xml.XPath // XPath query language
System.Xml.Xsl // Stylesheet support
System.Text.Json // JSON reader/writer and document object model
```

### Diagnostics

we cover logging and assertion and describe how to interact with other processes, write to the Windows event log, and handle performance monitoring. The types for this are defined in and under `System.Diagnostics`.

### Concurrency and Asynchrony

Many modern applications need to deal with more than one thing happening at a time. Since C# 5.0, this has become easier through asynchronous functions and high-level constructs such as tasks and task combinators.

Types for working with threads and asynchronous operations are in the `System.Threading` and
`System.Threading.Tasks` namespaces.

### Streams and I/O

.NET Core provides a stream-based model for low-level I/O. Streams are typically used to read and write directly to files and network connections, and can be chained or wrapped in decorator streams to add compression or encryption functionality. The Stream and I/O types are defined in and under the  [`System.IO`](http://system.io/) namespace

### Networking

You can directly access standard network protocols such as HTTP, FTP, TCP/IP, and
SMTP via the types in `System.Net`.

```csharp
System.Net
System.Net.Http // HttpClient
System.Net.Mail // For sending mail via SMTP
System.Net.Sockets // TCP, UDP, and IP
```

### Serialization

The Framework provides several systems for saving and restoring objects to a binary or text representation. Such systems can be required for communication as well as saving and restoring objects to a file.

we cover the three major serialization engines: the binary serializer, the JSON serializer, and the XML serializer. The types for serialization reside in the following namespaces.

```csharp
System.Runtime.Serialization
System.Xml.Serialization
System.Text.Json
```

### Assemblies, Reflection, and Attributes

The assemblies into which C# programs compile comprise executable instructions
(stored as IL) and metadata, which describes the program’s types, members, and
attributes. Through reflection, you can inspect this metadata at runtime and do such
things as dynamically invoke methods. With Reflection.Emit, you can construct
new code on the fly.

```csharp
System
System.Reflection
System.Reflection.Emit
```

### Dynamic Programming

we look at some of the patterns for dynamic programming and utilizing the Dynamic Language Runtime (DLR). We describe how to implement the Visitor pattern, write custom dynamic objects, and interoperate with IronPython. The types for dynamic programming are in `System.Dynamic`.

### Cryptography

.NET Core provides extensive support for popular hashing and encryption protocols.

```csharp
System.Security
System.Security.Cryptography
```

### Advanced Threading

C#’s asynchronous functions make concurrent programming significantly easier because they lessen the need for lower-level techniques. However, there are still times when you need signaling constructs, thread-local storage, reader/writer locks, and so on. Threading types are in the
`System.Threading` namespace.

### Parallel Programming

we cover in detail the libraries and types for leveraging multicore processors, including APIs for task parallelism, imperative data parallelism, and functional parallelism (PLINQ).

### Native and COM Interoperability

You can interoperate with both native and Component Object Model (COM) code. Native interoperability allows you to call functions in unmanaged DLLs, register callbacks, map data structures, and interoperate with native data types. COM interoperability allows you to call COM types (on Windows machines), and expose .NET Core types to COM. The types that support these functions are in `System.Runtime.InteropServices`

### The Roslyn Compiler

The C# compiler itself is written in C#—the project is called “Roslyn,” and the libraries are available as NuGet packages. With these libraries, you can utilize the compiler’s functionality in many ways besides compiling source code to an assembly, such as writing code analysis and refactoring tools. 

# Application Frameworks

UI–based applications can be divided into two categories: thin client, which amounts to a website, and rich client, which is a program the end user must download and install on a computer or mobile device.

For writing thin-client applications in C#, there’s ASP.NET Core, which runs on Windows, Linux, and macOS. ASP.NET Core is also designed for writing web APIs.

For rich-client applications, there are a choice of APIs

- The Windows Desktop framework includes the WPF and Windows Forms APIs, and runs on Windows 7/8/10 desktop
- UWP runs on Windows 10 desktop and devices
- Xamarin runs on iOS and Android mobile devices

### [ASP.NET](http://asp.net/) Core

ASP.NET Core is a lightweight modular successor to ASP.NET, with support for the popular Model-View-Controller (MVC) pattern. ASP.NET Core is suitable for creating websites, REST-based web APIs, and microservices. It can also run in conjunction with two popular single-page-application frameworks: React and Angular.

ASP.NET Core runs on Windows, Linux, and macOS and can self-host in a custom process. Unlike its predecessor (ASP.NET), ASP.NET Core is not dependent on `System.Web` and the historical baggage of web forms

As with any thin-client architecture, ASP.NET Core offers the following general advantages over rich clients:

- There is zero deployment at the client end
- The client can run on any platform that supports a web browser
- Updates are easily deployed

## Windows Desktop

The Windows Desktop application framework offers a choice of two UI APIs for writing rich-client applications: WPF and Windows Forms. Both APIs run on Windows Desktop/Server 7 through 10.

### WPF

WPF was introduced in 2006, and has been enhanced ever since. Unlike its predecessor, Windows Forms, WPF explicitly renders controls using DirectX, with the following benefits

- It supports sophisticated graphics, such as arbitrary transformations, **3D rendering**, multimedia, and **true transparency**. Skinning is supported through styles and templates.
- Its primary **measurement unit is not pixel-based, so applications display correctly at any DPI setting**.
- It has extensive and flexible layout support, which means that you can **localize an application without danger of elements overlapping**.
- Its use of **DirectX makes rendering fast and able to take advantage of graphics hardware acceleration.**
- It offers **reliable data binding**.
- UIs can be described declaratively in **XAML files that can be maintained independent of the “code-behind” files**—this helps to separate appearance from functionality.

<aside>
🔥 WPF takes some time to learn due to its size and complexity. The types for writing
WPF applications are in the `System.Windows` namespace and all subnamespaces
except for `System.Windows.Forms`.

</aside>

### Windows Forms

Windows Forms is a rich-client API that shipped with the first version of .NET Framework in 2000. **Compared to WPF, Windows Forms is a relatively simple technology that provides most of the features you need in writing a typical Windows application**. It also has significant relevancy in maintaining legacy applications.

- Although it provides mechanisms for DPI awareness, it’s still too easy to write applications that break on clients whose DPI settings differ from the developer’s.
- The API for drawing nonstandard controls is GDI+, which, although reasonably flexible, is slow in rendering large areas (and, without double buffering,might flicker).
- Controls **lack true transparency**.
- **Most controls are noncompositional. For instance, you can’t put an image control inside a tab control header. Customizing list views, combo boxes, and tab controls in a way that would be trivial with WPF is time consuming and painful in Windows Forms.**
- Dynamic layout is difficult to correctly implement.

<aside>
📢 The last point is an excellent reason to favor WPF over Windows Forms—even if you’re writing a business application that needs just a UI and not a “user experience.” The layout elements in WPF, such as Grid, make it easy to assemble labels and text boxes such that they always align—even after language-changing localization—without messy logic and without any flickering. Further, you don’t need to bow to the lowest common denominator in screen resolution—WPF layout elements have been designed from the outset to adapt properly to resizing. On the positive side, Windows Forms is relatively simple to learn and still has a good number of third-party controls.

</aside>

The Windows Forms types are in the `System.Windows.Forms` (in `System.Windows.Forms.dll`) and `System.Drawing` (in `System.Drawing.dll`) namespaces. The latter also contains the GDI+ types for drawing custom controls.

### UWP

UWP is a rich-client API for writing touch-first UIs that target Windows 10 desktop and devices. The word “Universal” refers to its ability to run on a range of Windows

10 devices, including Xbox, Surface Hub, and Hololens. However, it’s not compatible with earlier versions of Windows, including Windows 7 and Windows 8/8.1.

The UWP API uses XAML and is somewhat similar to WPF. Here are its key
differences

- The primary mode of distribution for UWP apps is the Windows Store.
- UWP apps run in a sandbox to lessen the threat of malware, which means that
they cannot perform tasks such as reading or writing arbitrary files, and they
cannot run with administrative elevation.
- UWP relies on WinRT types that are part of the operating system (Windows),
not the managed framework. This means that when writing apps, you must
nominate a Windows 10 version range (such as Windows 10 build 17763 to
Windows 10 build 18362). This means that you either need to target an old
API, or require that your customers install the latest Windows update.

<aside>
⚠️ To address the last point, Microsoft is introducing WinUI 3, which transfers the
WinRT APIs from the operating system to the framework. WinUI 3 will also help to
bridge the divide between Windows Desktop and UWP: rather than having to
choose one or the other, you’ll be able to mix and match components from each.

</aside>

The namespaces in UWP are `Windows.UI` and `Windows.UI.Xaml`.

### Xamarin

Xamarin lets you write mobile apps in C# that target iOS and Android. Xamarin doesn’t run on .NET Core, but on Mono (a derivation of the open source Mono framework).# Chapter 6 : Framework Fundamentals

# String and Text Handling

## char

A C# char represents a single Unicode character and aliases the System.

```csharp
char c = 'A';
char newLine = '\n';

// System.Char defines a range of static methods for working with characters:
Console.WriteLine (char.ToUpper ('c'));				// C
Console.WriteLine (char.IsWhiteSpace ('\t'));		// True
Console.WriteLine (char.IsLetter ('x'));			// True
Console.WriteLine (char.GetUnicodeCategory ('x'));	// LowercaseLetter
```

`System.Char` defines a range of static methods for working with characters, such as
`ToUpper`, `ToLower`, and `IsWhiteSpace`. 

## string

A C# string (== System.String) is an **immutable** (unchangeable) sequence of characters.

### Constructing strings

```csharp
// String literals:
string s1 = "Hello";
string s2 = "First Line\r\nSecond Line";
string s3 = @"\\server\fileshare\helloworld.cs";

// To create a repeating sequence of characters you can use string’s constructor:
Console.Write (new string ('*', 10));    // **********

// You can also construct a string from a char array. ToCharArray does the reverse:
char[] ca = "Hello".ToCharArray();
string s = new string (ca);              // s = "Hello"
s.Dump();
```

### Null and empty strings

An empty string has a length of zero. To create an empty string, you can use either a literal or the static string.Empty field; to test for an empty string, you can either perform an equality comparison or test its Length property

```csharp
// An empty string has a length of zero:
string empty = "";
Console.WriteLine (empty == "");              // True
Console.WriteLine (empty == string.Empty);    // True
Console.WriteLine (empty.Length == 0);        // True

//Because strings are reference types, they can also be null:
string nullString = null;
Console.WriteLine (nullString == null);        // True
Console.WriteLine (nullString == "");          // False
Console.WriteLine (string.IsNullOrEmpty (nullString));	// True
Console.WriteLine (nullString.Length == 0);             // NullReferenceException
```

### Accessing characters within a string

A string’s indexer returns a single character at the given index. As with all functions that operate on strings, this is zero-indexed

```csharp
string str  = "abcde";
char letter = str[1];        // letter == 'b'

// string also implements IEnumerable<char>, so you can foreach over its characters:
foreach (char c in "123") Console.Write (c + ",");    // 1,2,3,
```

### Searching within strings

The simplest methods for searching within strings are StartsWith, EndsWith, and Contains. These all return true or false

```csharp
// The simplest search methods are Contains, StartsWith, and EndsWith:
Console.WriteLine ("quick brown fox".Contains ("brown"));    // True
Console.WriteLine ("quick brown fox".EndsWith ("fox"));      // True

// IndexOf returns the first position of a given character or substring:
Console.WriteLine ("abcde".IndexOf ("cd"));   // 2
Console.WriteLine ("abcde".IndexOf ("xx"));   // -1

// IndexOf is overloaded to accept a startPosition StringComparison enum, which enables case-insensitive searches:
Console.WriteLine ("abcde".IndexOf ("CD", StringComparison.CurrentCultureIgnoreCase));    // 2

// LastIndexOf is like IndexOf, but works backward through the string.
// IndexOfAny returns the first matching position of any one of a set of characters:
Console.WriteLine ("ab,cd ef".IndexOfAny (new char[] {' ', ','} ));       // 2
Console.WriteLine ("pas5w0rd".IndexOfAny ("0123456789".ToCharArray() ));  // 3

// LastIndexOfAny does the same in the reverse direction.
```

### Manipulating strings

Because string is immutable, all the methods that manipulate a string return a new one, leaving the original untouched (the same goes for when you reassign a string variable).

```csharp
// Because String is immutable, all the methods below return a new string, leaving the original untouched.

// Substring extracts a portion of a string:
string left3 = "12345".Substring (0, 3);     // left3 = "123";
string mid3  = "12345".Substring (1, 3);     // mid3 = "234";

// If you omit the length, you get the remainder of the string:
string end3  = "12345".Substring (2);        // end3 = "345";

// Insert and Remove insert or remove characters at a specified position:
string s1 = "helloworld".Insert (5, ", ");    // s1 = "hello, world"
string s2 = s1.Remove (5, 2);                 // s2 = "helloworld";

// PadLeft and PadRight pad a string to a given length with a specified character (or a space if unspecified):
Console.WriteLine ("12345".PadLeft (9, '*'));  // ****12345
Console.WriteLine ("12345".PadLeft (9));       //     12345

// TrimStart, TrimEnd and Trim remove specified characters (whitespace, by default) from the string:
Console.WriteLine ("  abc \t\r\n ".Trim().Length);   // 3

// Replace replaces all occurrences of a particular character or substring:
Console.WriteLine ("to be done".Replace (" ", " | ") );  // to | be | done
Console.WriteLine ("to be done".Replace (" ", "")    );  // tobedone
```

### Splitting and joining strings

```csharp
// Split takes a sentence and returns an array of words (default delimiters = whitespace):
string[] words = "The quick brown fox".Split();
foreach (string word in words)
 Console.Write (word + "|"); // The|quick|brown|fox|

//The static Join method does the reverse of Split
string[] words = "The quick brown fox".Split();
string together = string.Join (" ", words); // The quick brown fox

//The static Concat method is similar to Join but accepts only a params string array
//and applies no separator. Concat is exactly equivalent to the + operator (the com‐
//piler, in fact, translates + to Concat)
string sentence     = string.Concat ("The", " quick", " brown", " fox");
string sameSentence = "The" + " quick" + " brown" + " fox";
```

### string.Format and composite format strings

The static Format method provides a convenient way to build strings that embed variables. The embedded variables (or values) can be of any type; the Format simply calls ToString on them.

Each number in curly braces is called a format item. The number corresponds to the
argument position and is optionally followed by

- A comma and a minimum width to apply
- A colon and a format string

```csharp
// When calling String.Format, provide a composite format string followed by each of the embedded variables
string composite = "It's {0} degrees in {1} on this {2} morning";
string s = string.Format (composite, 35, "Perth", DateTime.Now.DayOfWeek);
// s == "It's 35 degrees in Perth on this Friday morning"

//We can use interpolated string literals to the same effect
string s = $"It's hot this {DateTime.Now.DayOfWeek} morning";

// The minimum width in a format string is useful for aligning columns.
// If the value is negative, the data is left-aligned; otherwise, it’s right-aligned:
composite = "Name={0,-20} Credit Limit={1,15:C}";
Console.WriteLine (string.Format (composite, "Mary", 500));
Console.WriteLine (string.Format (composite, "Elizabeth", 20000));

//Name=Mary Credit Limit= $500.00
//Name=Elizabeth Credit Limit= $20,000.00

// The equivalent without using string.Format:
s = "Name=" + "Mary".PadRight (20) + " Credit Limit=" + 500.ToString ("C").PadLeft (15);
```

### Comparing Strings

In comparing two values, .NET Core differentiates the concepts of equality compari‐
son and order comparison. Equality comparison tests whether two instances are
semantically the same; order comparison tests which of two (if any) instances comes
first when arranging them in ascending or descending sequence.

For string equality comparison, you can use the == operator or one of string’s
Equals methods. The latter are more versatile because they allow you to specify
options such as case insensitivity.

Another difference is that == does not work reliably on strings if the variables are cast to the object type. 

```csharp
// String comparisons can be ordinal vs culture-sensitive; case-sensitive vs case-insensitive.

Console.WriteLine (string.Equals ("foo", "FOO", StringComparison.OrdinalIgnoreCase));   // True

// (The following symbols may not be displayed correctly, depending on your font):
Console.WriteLine ("ṻ" == "ǖ");   // False

// The order comparison methods return a positive number, a negative number, or zero, depending
// on whether the first value comes after, before, or alongside the second value:
Console.WriteLine ("Boston".CompareTo ("Austin"));    // 1
Console.WriteLine ("Boston".CompareTo ("Boston"));    // 0
Console.WriteLine ("Boston".CompareTo ("Chicago"));   // -1
Console.WriteLine ("ṻ".CompareTo ("ǖ"));              // 0
Console.WriteLine ("foo".CompareTo ("FOO"));          // -1
```

### StringBuilder

The StringBuilder class (System.Text namespace) represents a mutable (editable) string. With a StringBuilder, you can Append, Insert, Remove, and Replace substrings without replacing the whole StringBuilder.

StringBuilder’s constructor optionally accepts an initial string value as well as a starting size for its internal capacity (default is 16 characters). If you go beyond this, StringBuilder automatically resizes its internal structures to accommodate (at a slight performance cost) up to its maximum capacity (default is int.MaxValue).

A popular use of StringBuilder is to build up a long string by repeatedly calling Append. This approach is much more efficient than repeatedly concatenating ordinary string types

```csharp
// Unlike string, StringBuilder is mutable.

// The following is more efficient than repeatedly concatenating ordinary string types:

StringBuilder sb = new StringBuilder();

for (int i = 0; i < 50; i++) sb.Append (i + ",");

// To get the final result, call ToString():
Console.WriteLine (sb.ToString());
//0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,
//27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,

sb.Remove (0, 60);		// Remove first 50 characters
sb.Length = 10;			// Truncate to 10 characters
sb.Replace (",", "+");	// Replace comma with +
Console.WriteLine (sb.ToString());

sb.Length = 0;
```

AppendLine performs an Append that adds a newline sequence ("\r\n" in Windows). AppendFormat accepts a composite format string, just like string.Format.

<aside>
📢 Setting a StringBuilder’s Length to zero doesn’t shrink its internal capacity. So, if the StringBuilder previously contained one million characters, it will continue to occupy
around two megabytes of memory after zeroing its Length. If you want to release the memory, you must create a new StringBuilder and allow the old one to drop out of scope
(and be garbage-collected).

</aside>

# Dates and Times

Three immutable structs in the System namespace do the job of representing dates
and times: DateTime, DateTimeOffset, and TimeSpan.

### TimeSpan

A TimeSpan represents an interval of time—or a time of the day. In the latter role, it’s simply the “clock” time (without the date), which is equivalent to the time since midnight, assuming no daylight saving transition. A TimeSpan has a resolution of 100 ns, has a maximum value of about 10 million days, and can be positive or negative.

```csharp
// There are three ways to construct a TimeSpan:
//  • Through one of the constructors
//  • By calling one of the static From . . . methods
//  • By subtracting one DateTime from another

Console.WriteLine (new TimeSpan (2, 30, 0));     // 02:30:00
Console.WriteLine (TimeSpan.FromHours (2.5));    // 02:30:00
Console.WriteLine (TimeSpan.FromHours (-2.5));   // -02:30:00
Console.WriteLine (DateTime.MaxValue - DateTime.MinValue);

// TimeSpan overloads the < and > operators, as well as the + and - operators:

(TimeSpan.FromHours(2) + TimeSpan.FromMinutes(30)).Dump ("2.5 hours");
(TimeSpan.FromDays(10) - TimeSpan.FromSeconds(1)).Dump ("One second short of 10 days");
```

```csharp
//constructors for Time Span
public TimeSpan (int hours, int minutes, int seconds);
public TimeSpan (int days, int hours, int minutes, int seconds);
public TimeSpan (int days, int hours, int minutes, int seconds,
 int milliseconds);
public TimeSpan (long ticks); // Each tick = 100ns

//Methods
public static TimeSpan FromDays (double value);
public static TimeSpan FromHours (double value);
public static TimeSpan FromMinutes (double value);
public static TimeSpan FromSeconds (double value);
public static TimeSpan FromMilliseconds (double value);
```

### Constructing a DateTime

DateTime defines constructors that accept integers for the year, month, and day—
and optionally, the hour, minute, second, and millisecond

```csharp
public DateTime (int year, int month, int day);
public DateTime (int year, int month, int day,
 int hour, int minute, int second, int millisecond);
```

### The current DateTime/DateTimeOset

```csharp
Console.WriteLine (DateTime.Now);
Console.WriteLine (DateTimeOffset.Now);

Console.WriteLine (DateTime.Today);		// No time portion

Console.WriteLine (DateTime.UtcNow);
Console.WriteLine (DateTimeOffset.UtcNow);

/*
18-6-20 01:45:55 PM
18-6-20 01:45:55 PM +05:30
18-6-20 12:00:00 AM
18-6-20 08:15:55 AM
18-6-20 08:15:55 AM +00:00
*/
```

### Working with dates and times

```csharp
DateTime dt = new DateTime (2000, 2, 3,
                            10, 20, 30);

Console.WriteLine (dt.Year);         // 2000
Console.WriteLine (dt.Month);        // 2
Console.WriteLine (dt.Day);          // 3
Console.WriteLine (dt.DayOfWeek);    // Thursday
Console.WriteLine (dt.DayOfYear);    // 34

Console.WriteLine (dt.Hour);         // 10
Console.WriteLine (dt.Minute);       // 20
Console.WriteLine (dt.Second);       // 30
Console.WriteLine (dt.Millisecond);  // 0
Console.WriteLine (dt.Ticks);        // 630851700300000000
Console.WriteLine (dt.TimeOfDay);    // 10:20:30  (returns a TimeSpan)

TimeSpan ts = TimeSpan.FromMinutes (90);
Console.WriteLine (dt.Add (ts));         // 3/02/2000 11:50:30 AM
Console.WriteLine (dt + ts);             // 3/02/2000 11:50:30 AM

DateTime thisYear = new DateTime (2007, 1, 1);
DateTime nextYear = thisYear.AddYears (1);
TimeSpan oneYear = nextYear - thisYear;
```

# Formatting and Parsing

Formatting means converting to a string; parsing means converting from a string.
The need to format or parse arises frequently in programming, in a variety of situa‐
tions.

ToString and Parse

- These methods provide default functionality for many types

Format providers

- These manifest as additional ToString (and Parse) methods that accept a for‐
mat string and/or a format provider. Format providers are highly flexible and
culture-aware.

XmlConvert

- This is a static class with methods that format and parse while honoring XML
standards. XmlConvert is also useful for general-purpose conversion when you
need culture independence or you want to preempt misparsing. XmlConvert
supports the numeric types, bool, DateTime, DateTimeOffset, TimeSpan, and
Guid.

Type converters

- These target designers and XAML parsers.

### ToString and Parse

The simplest formatting mechanism is the ToString method. It gives meaningful output on all simple value types. For the reverse operation, each of these types defines a static Parse method

```csharp
string s = true.ToString(); // s = "True"
bool b = bool.Parse (s); // b = true
```

If the parsing fails, a FormatException is thrown. Many types also define a TryParse method, which returns false if the conversion fails rather than throwing an exception

```csharp
bool failure = int.TryParse ("qwerty", out int i1);
bool success = int.TryParse ("123", out int i2);
```

If you don’t care about the output and want to test only whether parsing would suc‐
ceed, you can use a discard:

```csharp
bool success = int.TryParse ("123", out int _);
```

### Format Providers

```csharp
// The format string provides instructions; the format provider determines how the instructions are translated:
NumberFormatInfo f = new NumberFormatInfo();
f.CurrencySymbol = "$$";
Console.WriteLine (3.ToString ("C", f));          // $$ 3.00

// The default format provider is CultureInfo.CurrentCulture:
Console.WriteLine (10.3.ToString ("C", null));

// For convenience, most types overload ToString such that you can omit a null provider:
Console.WriteLine (10.3.ToString ("C"));
Console.WriteLine (10.3.ToString ("F4"));		// (Fix to 4 D.P.)
```

### Composite formatting

Composite format strings allow you to combine variable substitution with format
strings. The static string.Format method accepts a composite format string

```csharp
string composite = "Credit={0:C}";
Console.WriteLine (string.Format (composite, 500));   // Credit=$500.00

Console.WriteLine ("Credit={0:C}", 500);   // Credit=$500.00
```

### Standard Format Strings

```csharp
Console.WriteLine (12.345.ToString ("F4"));//F=Fix 4=Decimal
Console.WriteLine (121.345.ToString ("E4")); 
Console.WriteLine (121.345.ToString ("C4")); 
Console.WriteLine (12.345.ToString (".##")); 
Console.WriteLine (12.345.ToString (".####")); 
Console.WriteLine (12.345.ToString (".0000")); 
Console.WriteLine (1234.5.ToString ("#,###,###.00")); 
Console.WriteLine (1234.5.ToString ("0,000,000")); 
Console.WriteLine (0.6.ToString ("00%")); 
Console.WriteLine (12.345.ToString ("0E0"));

/*
12.3450
1.2134E+002
₹ 121.3450
121.345
12.35
12.345
12.3450
1,234.50
00,01,235
60%
1E1
*/
```

# Globalization

There are two aspects to internationalizing an application: globalization and localization.

Globalization is concerned with three tasks (in decreasing order of importance)

- Making sure that your program doesn’t break when run in another culture
- Respecting a local culture’s formatting rules; for instance, when displaying
dates
- Designing your program so that it picks up culture-specific data and strings
from satellite assemblies that you can later write and deploy

### Globalization Checklist

- Understand Unicode and text encodings
- Be mindful that methods such as ToUpper and ToLower on char and string are
culture sensitive: use ToUpperInvariant/ToLowerInvariant unless you want
culture sensitivity
- Favor culture-independent formatting and parsing mechanisms for DateTime
and DateTimeOffsets such as ToString("o") and XmlConvert
- Otherwise, specify a culture when formatting/parsing numbers or date/times
(unless you want local-culture behavior).

# The IEquatable<T> interface

### Overriding Equals

In addition to overriding Equals, you can optionally overload the equality and inequality operators. This is nearly always done with structs because the consequence of not doing so is that the == and != operators will simply not work on your type.

With classes, there are two ways to proceed:

- Leave == and != alone—so that they apply referential equality
- Overload == and != in line with Equals.

The first approach is most common with custom types—especially mutable types. It ensures that your type follows the expectation that == and != should exhibit referential equality with reference types and this avoids confusing consumers.

```csharp
var sb1 = new StringBuilder ("foo");
var sb2 = new StringBuilder ("foo");
Console.WriteLine (sb1 == sb2); // False (referential equality)
Console.WriteLine (sb1.Equals (sb2)); // True (value equality)
```

### Implementing IEquatable<T>

Imagine that we need a struct to represent an area whose width and height are inter‐
changeable. In other words, 5 × 10 is equal to 10 × 5.

```csharp
public struct Area : IEquatable <Area>
{
 public readonly int Measure1;
 public readonly int Measure2;
 public Area (int m1, int m2)
 {
 Measure1 = Math.Min (m1, m2);
 Measure2 = Math.Max (m1, m2);
 }
 public override bool Equals (object other)
 {
 if (!(other is Area)) return false;
 return Equals ((Area) other); // Calls method below
 }
 public bool Equals (Area other) // Implements IEquatable<Area>
 => Measure1 == other.Measure1 && Measure2 == other.Measure2;
 public override int GetHashCode()
 => HashCode.Combine (Measure1, Measure2);
 public static bool operator == (Area a1, Area a2) => a1.Equals (a2);
 public static bool operator != (Area a1, Area a2) => !a1.Equals (a2);
}

Area a1 = new Area (5, 10);
Area a2 = new Area (10, 5);
Console.WriteLine (a1.Equals (a2)); // True
Console.WriteLine (a1 == a2); // True
```

# Utility Classes

### Console

The static Console class handles standard input/output for console-based applications. In a command-line (Console) application, the input comes from the keyboard via Read, ReadKey, and ReadLine, and the output goes to the text window via Write and WriteLine.

```csharp
Console.WindowWidth = Console.LargestWindowWidth;
Console.ForegroundColor = ConsoleColor.Green;
Console.Write ("test... 50%");
Console.CursorLeft -= 3;
Console.Write ("90%"); // test... 90%
```

The Write and WriteLine methods are overloaded to accept a composite format string

The Console.Out property returns a TextWriter. Passing Console.Out to a method
that expects a TextWriter is a useful way to get that method to write to the Console
for diagnostic purposes.

### Environment

The static System.Environment class provides a range of useful properties:

Files and folders

- CurrentDirectory, SystemDirectory, CommandLine

Computer and operating system

- MachineName, ProcessorCount, OSVersion, NewLine

User logon

- UserName, UserInteractive, UserDomainName

Diagnostics

- TickCount, StackTrace, WorkingSet, Version

### Process

The Process class in System.Diagnostics allows you to launch a new process

```csharp
Process.Start ("notepad.exe");
Process.Start ("notepad.exe", "e:\\file.txt");
```

The most flexible overload accepts a ProcessStartInfo instance. With this, you can capture and redirect the launched process’s input, output, and error output (if you leave UseShellExecute as false).

```csharp
ProcessStartInfo psi = new ProcessStartInfo
{
 FileName = "cmd.exe",
 Arguments = "/c ipconfig /all",
 RedirectStandardOutput = true,
 UseShellExecute = false
};
Process p = Process.Start (psi);
string result = p.StandardOutput.ReadToEnd();
Console.WriteLine (result);
```

### UseShellExecute

The UseShellExecute flag changes how the CLR starts the process. With Use ShellExecute true, you can do the following:

- Specify a path to a file or document rather than an executable (resulting in the operating system opening the file or document with its associated application)
- Specify a URL (resulting in the operating system navigating to that URL in the default web browser)
- (Windows only) Specify a Verb (such as runas, to run the process with administrative elevation)

```csharp
void Main()
{
    LaunchFileOrUrl ("http://www.albahari.com/nutshell");
}

void LaunchFileOrUrl (string url)
{
    if (RuntimeInformation.IsOSPlatform (OSPlatform.Linux))
        Process.Start ("xdg-open", url);
    else if (RuntimeInformation.IsOSPlatform (OSPlatform.Windows))
        Process.Start (new ProcessStartInfo (url) { UseShellExecute = true });
    else
        throw new NotSupportedException ("Platform unsupported.");
}
```# Chapter 7 : Collections

# Overview

.NET Core provides a standard set of types for storing and managing collections of objects. These include resizable lists, linked lists, sorted and unsorted dictionaries, as well as arrays. Of these, only arrays form part of the C# language; the remaining collections are just classes you instantiate like any other

We can divide the types in the Framework for collections into the following categories

- Interfaces that define standard collection protocols
- Ready-to-use collection classes (lists, dictionaries, etc.)
- Base classes for writing application-specific collections

The collection namespaces are as follows

- `System.Collections` Nongeneric collection classes and interfaces
- `System.Collections.Specialized` Strongly typed nongeneric collection classes
- `System.Collections.Generic` Generic collection classes and interfaces
- `System.Collections.ObjectModel` Proxies and bases for custom collections
- `System.Collections.Concurrent` Thread-safe collections (see Chapter 23)

# Enumeration

In computing, there are many different kinds of collections, ranging from simple data structures such as arrays or linked lists, to more complex ones such as red/black trees and hashtables. Although the internal implementation and external

characteristics of these data structures vary widely, the ability to traverse the contents of the collection is an almost universal need. The Framework supports this need via a pair of interfaces (IEnumerable, IEnumerator, and their generic counterparts) that allow different data structures to expose a common traversal API.

The IEnumerator interface defines the basic low-level protocol by which elements in a collection are traversed—or enumerated—in a forward-only manner. 

```csharp
public interface IEnumerator
{
 bool MoveNext();
 object Current { get; }
 void Reset();
}
```

MoveNext advances the current element or “cursor” to the next position, returning false if there are no more elements in the collection. Current returns the element at the current position (usually cast from object to a more specific type). MoveNext must be called before retrieving the first element—this is to allow for an empty collection. The Reset method, if implemented, moves back to the start, allowing the collection to be enumerated again. Reset exists mainly for Component Object Model (COM) interoperability; calling it directly is generally avoided because it’s not universally supported (and is unnecessary in that it’s usually just as easy to instantiate a new enumerator.)

Collections do not usually implement enumerators; instead, they provide enumera‐
tors, via the interface IEnumerable

```csharp
public interface IEnumerable
{
 IEnumerator GetEnumerator();
}
```

```csharp
string s = "Hello";

// Because string implements IEnumerable, we can call GetEnumerator():
IEnumerator rator = s.GetEnumerator();

while (rator.MoveNext())
{
    char c = (char) rator.Current;
    Console.Write (c + ".");
}

Console.WriteLine();

// Equivalent to:

foreach (char c in s)
    Console.Write (c + ".");
```

### IEnumerable<T> and IEnumerator<T>

IEnumerator and IEnumerable are nearly always implemented in conjunction with their extended generic versions

```csharp
public interface IEnumerator<T> : IEnumerator, IDisposable
{
 T Current { get; }
}
public interface IEnumerable<T> : IEnumerable
{
 IEnumerator<T> GetEnumerator();
}
```

By defining a typed version of Current and GetEnumerator, these interfaces strengthen static type safety, avoid the overhead of boxing with value-type elements, and are more convenient to the consumer. Arrays automatically implement IEnumerable (where T is the member type of the array).

### Implementing the Enumeration Interfaces

You might want to implement IEnumerable or IEnumerable for one or more of the following reasons

- To support the foreach statement
- To interoperate with anything expecting a standard collection
- To meet the requirements of a more sophisticated collection interface
- To support collection initializers

# The ICollection and IList Interfaces

Although the enumeration interfaces provide a protocol for forward-only iteration over a collection, they don’t provide a mechanism to determine the size of the collection, access a member by index, search, or modify the collection. For such functionality, the .NET Framework defines the ICollection, IList, and IDictionary interfaces. Each comes in both generic and nongeneric versions; however, the **non‐generic versions exist mostly for legacy support.**

- IEnumerable<T>(and IEnumerable) : Provides minimum functionality (enumeration only)
- ICollection<T> (and ICollection) : Provides medium functionality (e.g., the Count property)
- IList<T>/IDictionary<K,V> and their nongeneric versions :Provide maximum functionality (including “random” access by index/key)

It’s rare that you’ll need to implement any of these interfaces. In nearly all cases when you need to write a collection class, you can instead subclass Collection<T>. LINQ provides yet another option that covers many scenarios.

The generic and nongeneric versions differ in ways over and above what you might expect, particularly in the case of ICollection. The reasons for this are mostly historical: because generics came later, the generic interfaces were developed with the benefit of hindsight, leading to a different (and better) choice of members. For this reason, ICollection does not extend ICollection, IList does not extend IList, and IDictionary does not extend IDictionary.

### ICollection<T> and ICollection

ICollection<T> is the standard interface for countable collections of objects. It provides the ability to determine the size of a collection (Count), determine whether an item exists in the collection (Contains), copy the collection into an array (ToArray), and determine whether the collection is read-only (IsReadOnly). For writable collections, you can also Add, Remove, and Clear items from the collection. And because it extends IEnumerable, it can also be traversed via the foreach statement

```csharp
public interface ICollection<T> : IEnumerable<T>, IEnumerable
{
 int Count { get; }
 bool Contains(T item);
 void CopyTo(T[] array, int arrayIndex);
 bool IsReadOnly{ get; }
 void Add(T item);
 bool Remove(T item);
 void Clear();
}
```

### IList<T> and IList

IList<T> is the standard interface for collections indexable by position. In addition to the functionality inherited from ICollection and IEnumerable, it provides the ability to read or write an element by position (via an indexer) and insert/remove by position

```csharp
public interface IList<T> : ICollection<T>, IEnumerable<T>, IEnumerable
{
 T this [int index] { get; set; }
 int IndexOf (T item);
 void Insert (int index, T item);
 void RemoveAt (int index);
}
```

# The Array Class

Array implements the collection interfaces up to IList<T> in both their generic and nongeneric forms. IList<T> itself is implemented explicitly, though, to keep Array’s public interface clean of methods such as Add or Remove, which throw an exception on fixed-length collections such as arrays. The Array class does actually offer a static Resize method, although this works by creating a new array and then copying over each element. As well as being inefficient, references to the array elsewhere in the program will still point to the original version. A better solution for resizable collections is to use the List class

An array can contain value-type or reference-type elements. Value-type elements are stored in place in the array, so an array of three long integers (each 8 bytes) will occupy 24 bytes of contiguous memory. A reference-type element, however, occupies only as much space in the array as a reference

```csharp
StringBuilder[] builders = new StringBuilder [5];
builders [0] = new StringBuilder ("builder1");
builders [1] = new StringBuilder ("builder2");
builders [2] = new StringBuilder ("builder3");
long[] numbers = new long [3];
numbers [0] = 12345;
numbers [1] = 54321;
```

Because Array is a class, arrays are always (themselves) reference types—regardless of the array’s element type. This means that the statement arrayB = arrayA results in two variables that reference the same array. Similarly, two distinct arrays will always fail an equality test, unless you employ a structural equality comparer, which compares every element of the array

```csharp
object[] a1 = { "string", 123, true };
object[] a2 = { "string", 123, true };
Console.WriteLine (a1 == a2); // False
Console.WriteLine (a1.Equals (a2)); // False
IStructuralEquatable se1 = a1;
Console.WriteLine (se1.Equals (a2,
StructuralComparisons.StructuralEqualityComparer)); // True
```

Arrays can be duplicated by calling the Clone method: arrayB = arrayA.Clone(). However, this results in a shallow clone, meaning that only the memory represented by the array itself is copied. If the array contains value-type objects, the values themselves are copied; if the array contains reference-type objects, just the references are copied (resulting in two arrays whose members reference the same objects).

```csharp
StringBuilder[] builders2 = builders;
StringBuilder[] shallowClone = (StringBuilder[]) builders.Clone();
```

To create a deep copy—for which reference type subobjects are duplicated—you must loop through the array and clone each element manually. The same rules apply to other .NET collection types.

Many of the methods on the Array class that you expect to be instance methods are in fact static methods. This is an odd design decision, and means that you should check for both static and instance methods when looking for a method on Array.

### Construction and Indexing

```csharp
int[] myArray = { 1, 2, 3 };
int first = myArray [0];
int last = myArray [myArray.Length - 1];
```

Alternatively, you can instantiate an array dynamically by calling `Array.Create` Instance. This allows you to specify element type and rank (number of dimensions) at runtime as well as allowing nonzero-based arrays through specifying a lower bound. Nonzero-based arrays are not compatible with the .NET Common Language Specification (CLS) and should not be exposed as public members in a library that might be consumed by a program written in F# or Visual Basic.

```csharp
// Create a string array 2 elements in length:
 Array a = Array.CreateInstance (typeof(string), 2);
 a.SetValue ("hi", 0); // → a[0] = "hi";
 a.SetValue ("there", 1); // → a[1] = "there";
 string s = (string) a.GetValue (0); // → s = a[0];
 // We can also cast to a C# array as follows:
 string[] cSharpArray = (string[]) a;
 string s2 = cSharpArray [0];
```

```csharp
void WriteFirstValue (Array a)
 {
 Console.Write (a.Rank + "-dimensional; ");
 // The indexers array will automatically initialize to all zeros, so
 // passing it into GetValue or SetValue will get/set the zero-based
 // (i.e., first) element in the array.
 int[] indexers = new int[a.Rank];
 Console.WriteLine ("First value is " + a.GetValue (indexers));
 }
 void Demo()
 {
 int[] oneD = { 1, 2, 3 };
 int[,] twoD = { {5,6}, {8,9} };
 WriteFirstValue (oneD); // 1-dimensional; first value is 1
 WriteFirstValue (twoD); // 2-dimensional; first value is 5
 }
```

### Enumeration

```csharp
int[] myArray = { 1, 2, 3 };
foreach (int val in myArray)
 Console.WriteLine (val);
```

You can also enumerate using the static Array.ForEach method

```csharp
public static void ForEach<T> (T[] array, Action<T> action);
Array.ForEach (new[] { 1, 2, 3 }, Console.WriteLine);
```

### Length and Rank

Array provides the following methods and properties for querying length and rank:

```csharp
//GetLength and GetLongLength return the length for a given dimension
public int GetLength (int dimension);
public long GetLongLength (int dimension);
public int Length { get; }
public long LongLength { get; }
//GetLowerBound and GetUpperBound are useful with nonzero-indexed arrays.
public int GetLowerBound (int dimension);
public int GetUpperBound (int dimension);
public int Rank { get; } // Returns number of dimensions in array
```

### Searching

The Array class offers a range of methods for finding elements within a onedimensional array

- BinarySearch methods : For rapidly searching a sorted array for a particular item
- IndexOf/LastIndex methods : For searching unsorted arrays for a particular item
- Find/FindLast/FindIndex/FindLastIndex/FindAll/Exists/TrueForAll : For searching unsorted arrays for item(s) that satisfy a given Predicate<T>

None of the array-searching methods throws an exception if the specified value is not found. Instead, if an item is not found, methods returning an integer return −1 (assuming a zero-indexed array), and methods returning a generic type return the type’s default value (e.g., 0 for an int, or null for a string).

```csharp
static void Main()
{
 string[] names = { "Rodney", "Jack", "Jill" };
 string match = Array.Find (names, ContainsA);
 Console.WriteLine (match); // Jack
}
static bool ContainsA (string name) { return name.Contains ("a"); }
```

```csharp
//Here’s the same code shortened with an anonymous method:
string[] names = { "Rodney", "Jack", "Jill" };
string match = Array.Find (names, delegate (string name)
 { return name.Contains ("a"); } );
```

```csharp
//A lambda expression shortens it further:
string[] names = { "Rodney", "Jack", "Jill" };
string match = Array.Find (names, n => n.Contains ("a")); // Jack
string[] match = Array.FindAll (names, n => n.Contains ("a"))
```

### Sorting

```csharp
// For sorting a single array:
int[] numbers = { 3, 2, 1 };
Array.Sort (numbers); // Array is now { 1, 2, 3 }

// For sorting a pair of arrays:
int[] numbers = { 3, 2, 1 };
string[] words = { "three", "two", "one" };
Array.Sort (numbers, words);

// numbers array is now { 1, 2, 3 }
// words array is now { "one", "two", "three" }

// Sort such that odd numbers come first:
int[] numbers = { 1, 2, 3, 4, 5 };
Array.Sort (numbers, (x, y) => x % 2 == y % 2 ? 0 : x % 2 == 1 ? -1 : 1);
```

### Reversing Elements

```csharp
public static void Reverse (Array array);
public static void Reverse (Array array, int index, int length);
```

### Copying

- Clone
    - returns a whole new (shallow-copied) array
- CopyTo and Copy
    - copy a contiguous subset of the array
    - Copying a multidimensional rectangular array requires you to map the multidimensional index to a linear index.
- ConstrainedCopy
    - performs an atomic operation: if all of the requested elements
    cannot be successfully copied (due to a type error, for instance), the operation is rolled back

### Converting and Resizing

Array.ConvertAll creates and returns a new array of element type TOutput, calling the supplied Converter delegate to copy over the elements. Converter is defined as follows

```csharp
float[] reals = { 1.3f, 1.5f, 1.8f };
int[] wholes = Array.ConvertAll (reals, r => Convert.ToInt32 (r));
// wholes array is { 1, 2, 2 }
```

# Lists, Queues, Stacks, and Sets

.NET Core provides a basic set of concrete collection classes that implement the interfaces described in this chapter. This section concentrates on the list-like collections.

### List<T> and ArrayList

The generic List and nongeneric ArrayList classes provide a dynamically sized array of objects and are among the most commonly used of the collection classes. ArrayList implements IList, whereas List implements both IList and IList<T> . All interfaces are implemented publicly, and methods such as Add and Remove are exposed and work as you would expect.

Internally, List and ArrayList work by maintaining an internal array of objects, replaced with a larger array upon reaching capacity. Appending elements is efficient (because there is usually a free slot at the end), but inserting elements can be slow (because all elements after the insertion point must be shifted to make a free slot), as can removing elements (especially near the start). As with arrays, searching is efficient if the BinarySearch method is used on a list that has been sorted, but it is
otherwise inefficient because each item must be individually checked.

List is up to several times faster than ArrayList if T is a value type, because List avoids the overhead of boxing and unboxing elements.

```csharp
List<string> words = new List<string>();    // New string-typed list

words.Add ("melon");
words.Add ("avocado");
words.AddRange (new[] { "banana", "plum" } );
words.Insert (0, "lemon");                           // Insert at start
words.InsertRange (0, new[] { "peach", "nashi" });   // Insert at start

words.Remove ("melon");
words.RemoveAt (3);                         // Remove the 4th element
words.RemoveRange (0, 2);                   // Remove first 2 elements

// Remove all strings starting in 'n':
words.RemoveAll (s => s.StartsWith ("n"));

Console.WriteLine (words [0]);                          // first word
Console.WriteLine (words [words.Count - 1]);            // last word
foreach (string s in words) Console.WriteLine (s);      // all words
List<string> subset = words.GetRange (1, 2);            // 2nd->3rd words

string[] wordsArray = words.ToArray();    // Creates a new typed array

// Copy first two elements to the end of an existing array:
string[] existing = new string [1000];
words.CopyTo (0, existing, 998, 2);

List<string> upperCaseWords = words.ConvertAll (s => s.ToUpper());
List<int> lengths = words.ConvertAll (s => s.Length);
```

### LinkedList<T>

LinkedList<T> is a generic doubly linked list. A doubly linked list is a chain of nodes in which each references the node before, the node after, and the actual element. Its main benefit is that an element can always be inserted efficiently anywhere in the list because it just involves creating a new node and updating a few references. However, finding where to insert the node in the first place can be slow because there’s no intrinsic mechanism to index directly into a linked list; each node must be traversed, and binary-chop searches are not possible.

```csharp
var tune = new LinkedList<string>();
tune.AddFirst ("do");                           tune.Dump(); // do
tune.AddLast ("so");                            tune.Dump(); // do - so

tune.AddAfter (tune.First, "re");               tune.Dump(); // do - re- so
tune.AddAfter (tune.First.Next, "mi");          tune.Dump(); // do - re - mi- so
tune.AddBefore (tune.Last, "fa");               tune.Dump(); // do - re - mi - fa- so

tune.RemoveFirst();                             tune.Dump(); // re - mi - fa - so
tune.RemoveLast();                              tune.Dump(); // re - mi - fa

LinkedListNode<string> miNode = tune.Find ("mi");
tune.Remove (miNode);                           tune.Dump(); // re - fa
tune.AddFirst (miNode);                         tune.Dump(); // mi- re - fa
```

# Queue<T> and Queue

Queue and Queue are first-in, first-out (FIFO) data structures, providing methods to Enqueue (add an item to the tail of the queue) and Dequeue (retrieve and remove the item at the head of the queue). A Peek method is also provided to return the element at the head of the queue without removing it, and there is a Count property (useful in checking that elements are present before dequeuing).

```csharp
var q = new Queue<int>();
q.Enqueue (10);
q.Enqueue (20);
int[] data = q.ToArray();         // Exports to an array
Console.WriteLine (q.Count);      // "2"
Console.WriteLine (q.Peek());     // "10"
Console.WriteLine (q.Dequeue());  // "10"
Console.WriteLine (q.Dequeue());  // "20"
Console.WriteLine (q.Dequeue());  // throws an exception (queue empty)
```

# Stack<T> and Stack

Stack<T> and Stack are last-in, first-out (LIFO) data structures, providing methods to Push (add an item to the top of the stack) and Pop (retrieve and remove an element from the top of the stack). A nondestructive Peek method is also provided, as is a Count property and a ToArray method for exporting the data for random access:

```csharp
//The following example demonstrates Stack<int>:
var s = new Stack<int>();
s.Push (1); // Stack = 1
s.Push (2); // Stack = 1,2
s.Push (3); // Stack = 1,2,3
Console.WriteLine (s.Count); // Prints 3
Console.WriteLine (s.Peek()); // Prints 3, Stack = 1,2,3
Console.WriteLine (s.Pop()); // Prints 3, Stack = 1,2
Console.WriteLine (s.Pop()); // Prints 2, Stack = 1
Console.WriteLine (s.Pop()); // Prints 1, Stack = <empty>
Console.WriteLine (s.Pop()); // Throws exception
```

# BitArray

A BitArray is a dynamically sized collection of compacted bool values. It is more memory efficient than both a simple array of bool and a generic List of bool because it uses only one bit for each value, whereas the bool type otherwise occupies one byte for each value.

```csharp
var bits = new BitArray(2);
bits[1] = true;
bits.Xor (bits);               // Bitwise exclusive-OR bits with itself
Console.WriteLine (bits[1]);   // False
```

# HashSet<T> and SortedSet<T>

HashSet and SortedSet are generic collections new to Framework 3.5 and 4.0, respectively. Both have the following distinguishing features

- Their Contains methods execute quickly using a hash-based lookup.
- They do not store duplicate elements and silently ignore requests to add duplicates.
- You cannot access an element by position.

SortedSet<T> keeps elements in order, whereas HashSet<T> does not.

```csharp
{
    var letters = new HashSet<char> ("the quick brown fox");

    Console.WriteLine (letters.Contains ('t'));      // true
    Console.WriteLine (letters.Contains ('j'));      // false

    foreach (char c in letters) Console.Write (c);   // the quickbrownfx
}
Console.WriteLine();
{
    var letters = new SortedSet<char> ("the quick brown fox");

    foreach (char c in letters) 
        Console.Write (c);                                    //  bcefhiknoqrtuwx
        
    Console.WriteLine();

    foreach (char c in letters.GetViewBetween ('f', 'i'))
        Console.Write (c);                                    //  fhi
}
```

```csharp
{
    var letters = new HashSet<char> ("the quick brown fox");
    letters.IntersectWith ("aeiou");
    foreach (char c in letters) Console.Write (c);     // euio
}
Console.WriteLine();
{
    var letters = new HashSet<char> ("the quick brown fox");
    letters.ExceptWith ("aeiou");
    foreach (char c in letters) Console.Write (c);     // th qckbrwnfx
}
Console.WriteLine();
{
    var letters = new HashSet<char> ("the quick brown fox");
    letters.SymmetricExceptWith ("the lazy brown fox");
    foreach (char c in letters) Console.Write (c);     // quicklazy
}
```

# Dictionaries

A dictionary is a collection in which each element is a key/value pair. Dictionaries are most commonly used for lookups and sorted lists.

The Framework defines a standard protocol for dictionaries, via the interfaces IDictionary and IDictionary as well as a set of general-purpose dictionary classes.

The classes each differ in the following regard

- Whether or not items are stored in sorted sequence
- Whether or not items can be accessed by position (index) as well as by key
- Whether generic or nongeneric
- Whether it’s fast or slow to retrieve items by key from a large dictionary

### IDictionary<TKey,TValue>

Dictionary defines the standard protocol for all key/value-based collections. It extends ICollection by adding methods and properties to access elements based on a key of arbitrary type:

```csharp
public interface IDictionary <TKey, TValue> :
 ICollection <KeyValuePair <TKey, TValue>>, IEnumerable
{
 bool ContainsKey (TKey key);
 bool TryGetValue (TKey key, out TValue value);
 void Add (TKey key, TValue value);
 bool Remove (TKey key);
 TValue this [TKey key] { get; set; } // Main indexer - by key
 ICollection <TKey> Keys { get; } // Returns just keys
 ICollection <TValue> Values { get; } // Returns just values
}
```

To add an item to a dictionary, you either call Add or use the index’s set accessor the latter adds an item to the dictionary if the key is not already present (or updates the item if it is present). Duplicate keys are forbidden in all dictionary implementations, so calling Add twice with the same key throws an exception.

To retrieve an item from a dictionary, use either the indexer or the TryGetValue method. If the key doesn’t exist, the indexer throws an exception, whereas TryGetValue returns false. You can test for membership explicitly by calling Contains Key; however, this incurs the cost of two lookups if you subsequently retrieve the item.

### Dictionary<TKey,TValue> and Hashtable

The generic Dictionary class is one of the most commonly used collections (along with the List collection). It uses a hashtable data structure to store keys and values, and it is fast and efficient.

```csharp
var d = new Dictionary<string, int>();

d.Add("One", 1);
d["Two"] = 2;     // adds to dictionary because "two" not already present
d["Two"] = 22;    // updates dictionary because "two" is now present
d["Three"] = 3;

Console.WriteLine (d["Two"]);                // Prints "22"
Console.WriteLine (d.ContainsKey ("One"));   // true (fast operation)
Console.WriteLine (d.ContainsValue (3));     // true (slow operation)
int val = 0;
if (!d.TryGetValue ("onE", out val))
    Console.WriteLine ("No val");            // "No val" (case sensitive)

// Three different ways to enumerate the dictionary:

foreach (KeyValuePair<string, int> kv in d)          //  One; 1
    Console.WriteLine (kv.Key + "; " + kv.Value);    //  Two; 22
                                                                                                         //  Three; 3

foreach (string s in d.Keys) Console.Write (s);      // OneTwoThree
Console.WriteLine();
foreach (int i in d.Values) Console.Write (i);       // 1223

var dIgnoreCase = new Dictionary<string, bool> (StringComparer.OrdinalIgnoreCase);
dIgnoreCase["foo"] = true;
Console.WriteLine(dIgnoreCase["FOO"]);
```

### Sorted Dictionaries

The Framework provides two dictionary classes internally structured such that their content is always sorted by key:

- SortedDictionary
- SortedList1

SortedDictionary<,> is much faster than SortedList<,> at inserting elements in a random sequence (particularly with large lists). SortedList<,>, however, has an extra ability: to access items by index as well as by key. With a sorted list, you can go directly to the nth element in the sorting sequence (via the indexer on the Keys/ Values properties). To do the same with a SortedDictionary<,>, you must manually enumerate over n items.

```csharp
// MethodInfo is in the System.Reflection namespace

var sorted = new SortedList <string, MethodInfo>();

foreach (MethodInfo m in typeof (object).GetMethods())
    sorted [m.Name] = m;

sorted.Keys.Dump ("keys");
sorted.Values.Dump ("values");

foreach (MethodInfo m in sorted.Values)
    Console.WriteLine (m.Name + " returns a " + m.ReturnType);

Console.WriteLine (sorted ["GetHashCode"]);      // Int32 GetHashCode()

Console.WriteLine (sorted.Keys  [sorted.Count - 1]);            // ToString
Console.WriteLine (sorted.Values[sorted.Count - 1].IsVirtual);  // True
```

# Customizable Collections and Proxies

The collection classes discussed in previous sections are convenient in that you can directly instantiate them, but they don’t allow you to control what happens when an item is added to or removed from the collection. With strongly typed collections in an application, you sometimes need this control.

- To fire an event when an item is added or removed
- To update properties because of the added or removed item
- To detect an “illegal” add/remove operation and throw an exception (for example, if the operation violates a business rule)

The .NET Framework provides collection classes for this exact purpose, in the System.Collections.ObjectModel namespace. These are essentially proxies or wrappers that implement IList or IDictionary<,> by forwarding the methods through to an underlying collection. Each Add, Remove, or Clear operation is routed via a virtual method that acts as a “gateway” when overridden.

```csharp
public class Animal
{
    public string Name;
    public int Popularity;

    public Animal (string name, int popularity)
    {
        Name = name; Popularity = popularity;
    }
}

public class AnimalCollection : Collection <Animal>
{
    // AnimalCollection is already a fully functioning list of animals.
    // No extra code is required.
}

public class Zoo   // The class that will expose AnimalCollection.
{                  // This would typically have additional members.

    public readonly AnimalCollection Animals = new AnimalCollection();
}

static void Main()
{
    Zoo zoo = new Zoo();
    zoo.Animals.Add (new Animal ("Kangaroo", 10));
    zoo.Animals.Add (new Animal ("Mr Sea Lion", 20));
    foreach (Animal a in zoo.Animals) Console.WriteLine (a.Name);
}
```

As it stands, AnimalCollection is no more functional than a simple List<Animal>; its role is to provide a base for future extension. To illustrate, let’s now add a Zoo property to Animal so that it can reference the Zoo in which it lives and override each of the virtual methods in Collection to maintain that property automatically:

```csharp
public class Animal
{
    public string Name;
    public int Popularity;
    
    public Zoo Zoo { get; internal set; }

    public Animal (string name, int popularity)
    {
        Name = name; Popularity = popularity;
    }
}

public class AnimalCollection : Collection <Animal>
{
    Zoo zoo;
    public AnimalCollection (Zoo zoo) { this.zoo = zoo; }

    protected override void InsertItem (int index, Animal item)
    {
        base.InsertItem (index, item);
        item.Zoo = zoo;
    }
    protected override void SetItem (int index, Animal item)
    {
        base.SetItem (index, item);
        item.Zoo = zoo;
    }
    protected override void RemoveItem (int index)
    {
        this [index].Zoo = null;
        base.RemoveItem (index);
    }
    protected override void ClearItems()
    {
        foreach (Animal a in this) a.Zoo = null;
        base.ClearItems();
    }
}

public class Zoo
{
    public readonly AnimalCollection Animals;
    public Zoo() { Animals = new AnimalCollection (this); }
}

static void Main()
{
    Zoo zoo = new Zoo();
    zoo.Animals.Add (new Animal ("Kangaroo", 10));
    zoo.Animals.Add (new Animal ("Mr Sea Lion", 20));
    foreach (Animal a in zoo.Animals) Console.WriteLine (a.Name);
}
```

### KeyedCollection<TKey,TItem> and DictionaryBase

It both adds and subtracts functionality. What it adds is the ability to access items by key, much like
with a dictionary. What it subtracts is the ability to proxy your own inner list.

```csharp
public class Animal
{
    string name;
    public string Name
    {
        get { return name; }
        set {
            if (Zoo != null) Zoo.Animals.NotifyNameChange (this, value);
            name = value;
        }
    }
    public int Popularity;
    public Zoo Zoo { get; internal set; }

    public Animal (string name, int popularity)
    {
        Name = name; Popularity = popularity;
    }
}

public class AnimalCollection : KeyedCollection <string, Animal>
{
    Zoo zoo;
    public AnimalCollection (Zoo zoo) { this.zoo = zoo; }

    internal void NotifyNameChange (Animal a, string newName)
    {
        this.ChangeItemKey (a, newName);
    }

    protected override string GetKeyForItem (Animal item)
    {
        return item.Name;
    }

    protected override void InsertItem (int index, Animal item)
    {
        base.InsertItem (index, item);
        item.Zoo = zoo;
    }
    protected override void SetItem (int index, Animal item)
    {
        base.SetItem (index, item);
        item.Zoo = zoo;
    }
    protected override void RemoveItem (int index)
    {
        this [index].Zoo = null;
        base.RemoveItem (index);
    }
    protected override void ClearItems()
    {
        foreach (Animal a in this) a.Zoo = null;
        base.ClearItems();
    }
}

public class Zoo
{
    public readonly AnimalCollection Animals;
    public Zoo() { Animals = new AnimalCollection (this); }
}

static void Main()
{
    Zoo zoo = new Zoo();
    zoo.Animals.Add (new Animal ("Kangaroo", 10));
    zoo.Animals.Add (new Animal ("Mr Sea Lion", 20));
    Console.WriteLine (zoo.Animals [0].Popularity);               // 10
    Console.WriteLine (zoo.Animals ["Mr Sea Lion"].Popularity);   // 20
    zoo.Animals ["Kangaroo"].Name = "Mr Roo";
    Console.WriteLine (zoo.Animals ["Mr Roo"].Popularity);        // 10
}
```

### ReadOnlyCollection<T>

ReadOnlyCollection is a wrapper, or proxy, that provides a read-only view of a collection. This is 
useful in allowing a class to publicly expose read-only access to a collection that the class can still update internally. 
A read-only collection accepts the input collection in its constructor, to which it maintains a permanent reference. It doesn’t take a static copy of the input collection, so subsequent changes to the input collection are visible through the read-only wrapper.

```csharp
public class Test
{
    List<string> names;
    public ReadOnlyCollection<string> Names { get; private set; }

    public Test()
    {
        names = new List<string>();
        Names = new ReadOnlyCollection<string> (names);
    }

    public void AddInternally() { names.Add ("test"); }
}

void Main()
{
    Test t = new Test();

    Console.WriteLine (t.Names.Count);       // 0
    t.AddInternally();
    Console.WriteLine (t.Names.Count);       // 1
    
    t.Names.Add ("test");                    // Compiler error
    ((IList<string>) t.Names).Add ("test");  // NotSupportedException
}
```

# Immutable Collections

The immutable collections extend this principle, by providing collections that cannot be modified at all after initialization. Should you need to add an item to an immutable collection, you must instantiate a new collection, leaving the old one untouched.

Immutability is a hallmark of functional programming and has the following benefits

- It eliminates a large class of bugs associated with changing state.
- It vastly simplifies parallelism and multithreading, by avoiding most of the thread-safety problems
- It makes code easier to reason about.

The disadvantage of immutability is that when you need to make a change, you must create a whole new object. This incurs a performance hit, although there are mitigating strategies that we discuss in this section, including the ability to reuse portions of the original structure.

### Creating Immutable Collections

Each immutable collection type offers a Create() method, which accepts optional initial values and returns an initialized immutable collection

```csharp
ImmutableArray<int> array = ImmutableArray.Create<int> (1, 2, 3);
var list = new[] { 1, 2, 3 }.ToImmutableList();
```

### Manipulating Immutable Collections

```csharp
var oldList = ImmutableList.Create<int> (1, 2, 3);

ImmutableList<int> newList = oldList.Add (4);

Console.WriteLine (oldList.Count);     // 3  (unaltered)
Console.WriteLine (newList.Count);     // 4

var anotherList = oldList.AddRange (new[] { 4, 5, 6 });
```

### Builders

```csharp
ImmutableArray<int>.Builder builder = ImmutableArray.CreateBuilder<int>();
builder.Add(1);
builder.Add(2);
builder.Add(3);
builder.RemoveAt(0);
ImmutableArray<int> myImmutable = builder.ToImmutable();

myImmutable.Dump();

var builder2 = myImmutable.ToBuilder();
builder2.Add (4);      // Efficient
builder2.Remove (2);   // Efficient
// ...                  // More changes to builder...
// Return a new immutable collection with all the changes applied:
ImmutableArray<int> myImmutable2 = builder2.ToImmutable().Dump();
```

### Immutable Collections and Performance

Most of the immutable collections use an AVL tree internally, which allows the add/remove operations to reuse portions of the original internal structure rather than having to re-create the entire thing from scratch. This reduces the overhead of add/remove operations from potentially huge (with large collections) to just moderately large, but it comes at the cost of making read operations slower. The end result is that most immutable collections are slower than their mutable counterparts for both reading and writing.

The most seriously affected is ImmutableList , which for both read and add operations is 10 to 200 times slower than List (depending on the size of the list). This is why ImmutableArray exists: by using an array inside, it avoids the overhead for read operations (for which it’s comparable in performance to an ordinary mutable array). The flip side is that it’s much slower than (even) Immutable List for add operations because none of the original structure can be reused.

Calling Remove on an ImmutableArray is more expensive than calling Remove on a List even in the worst-case scenario of removing the first element—because allocating the new collection places additional load on the garbage collector.

Although the immutable collections as a whole incur a potentially significant performance cost, it’s important to keep the overall magnitude in perspective. An Add operation on an ImmutableList with a million elements is still likely to occur in less than a microsecond on a typical laptop, and a read operation, in less than 100 nanoseconds. And, if you need to perform write operations in a loop, you can avoid the accumulated cost with a builder.

The following factors also work to mitigate the costs

- Immutability allows for easy concurrency and parallelization (Chapter 23), so you can employ all available cores. Parallelizing with mutable state easily leads to errors, and requires the use of locks or concurrent collections, both of which hurt performance.
- With immutability, you don’t need to “defensively copy” collections or data structures to guard against unexpected change. This was a factor in favoring the use of immutable collections in writing recent portions of Visual Studio.
- In most typical programs, few collections have enough items for the difference to matter

# Plugging in Equality and Order

IEqualityComparer and IEqualityComparer<T>

- Performs plug-in equality comparison and hashing
- Recognized by Hashtable and Dictionary

IComparer and IComparer<T>

- Performs plug-in order comparison
- Recognized by the sorted dictionaries and collections; also, `Array.Sort`

```csharp
public class Customer
{
    public string LastName;
    public string FirstName;

    public Customer (string last, string first)
    {
        LastName = last;
        FirstName = first;
    }
}
public class LastFirstEqComparer : EqualityComparer <Customer>
{
    public override bool Equals (Customer x, Customer y)
        => x.LastName == y.LastName && x.FirstName == y.FirstName;

    public override int GetHashCode (Customer obj)
        => (obj.LastName + ";" + obj.FirstName).GetHashCode();
}

void Main()
{
    Customer c1 = new Customer ("Bloggs", "Joe");
    Customer c2 = new Customer ("Bloggs", "Joe");
    
    Console.WriteLine (c1 == c2);               // False
    Console.WriteLine (c1.Equals (c2));         // False
    
    var d = new Dictionary<Customer, string>();
    d [c1] = "Joe";
    Console.WriteLine (d.ContainsKey (c2));         // False

    var eqComparer = new LastFirstEqComparer();
    d = new Dictionary<Customer, string> (eqComparer);
    d [c1] = "Joe";
    Console.WriteLine (d.ContainsKey (c2));         // True
}
```

### IComparer and Comparer

Comparers are used to switch in custom ordering logic for sorted dictionaries and
collections

```csharp
class Wish
{
    public string Name;
    public int Priority;

    public Wish (string name, int priority)
    {
        Name = name;
        Priority = priority;
    }
}

class PriorityComparer : Comparer <Wish>
{
    public override int Compare (Wish x, Wish y)
    {
        if (object.Equals (x, y)) return 0;          // Fail-safe check
        return x.Priority.CompareTo (y.Priority);
    }
}

void Main()
{
    var wishList = new List<Wish>();
    wishList.Add (new Wish ("Peace", 2));
    wishList.Add (new Wish ("Wealth", 3));
    wishList.Add (new Wish ("Love", 2));
    wishList.Add (new Wish ("3 more wishes", 1));
    
    wishList.Sort (new PriorityComparer());
    wishList.Dump();
}
```

### StringComparer

`StringCompare`is a predefined plug-in class for equating and comparing strings, allowing you to specify language and case sensitivity. `StringComparer` implements both `IEqualityComparer` and `IComparer` (and their generic versions), so you can use it with any type of dictionary or sorted collection.

```csharp
var dict = new Dictionary<string, int> (StringComparer.OrdinalIgnoreCase);
dict["joe"] = 12345;
dict["JOE"].Dump();

string[] names = { "Tom", "HARRY", "sheila" };
CultureInfo ci = new CultureInfo ("en-AU");
Array.Sort<string> (names, StringComparer.Create (ci, false));
names.Dump();
```

### IStructuralEquatable and IStructuralComparable

Structs implement structural comparison by default: two structs are equal if all of their fields are equal. Sometimes, however, structural equality and order comparison are useful as plug-in options on other types, as well—such as arrays.

```csharp
{
    int[] a1 = { 1, 2, 3 };
    int[] a2 = { 1, 2, 3 };
    IStructuralEquatable se1 = a1;
    Console.WriteLine (a1.Equals (a2));                                  // False
    Console.WriteLine (se1.Equals (a2, EqualityComparer<int>.Default));  // True
}
{
    string[] a1 = "the quick brown fox".Split();
    string[] a2 = "THE QUICK BROWN FOX".Split();
    IStructuralEquatable se1 = a1;
    bool isTrue = se1.Equals (a2, StringComparer.InvariantCultureIgnoreCase);
}
{
    var t1 = Tuple.Create (1, "foo");
    var t2 = Tuple.Create (1, "FOO");
    IStructuralEquatable se1 = t1;
    Console.WriteLine (se1.Equals (t2, StringComparer.InvariantCultureIgnoreCase));     // True
    IStructuralComparable sc1 = t1;
    Console.WriteLine (sc1.CompareTo (t2, StringComparer.InvariantCultureIgnoreCase));  // 0
}
{
    var t1 = Tuple.Create (1, "FOO");
    var t2 = Tuple.Create (1, "FOO");
    Console.WriteLine (t1.Equals (t2));   // True
}
```# Chapter 8 : LINQ Queries

# Overview

`LINQ`, or Language-Integrated Query, is a set of language and framework features for writing structured type-safe queries over local object collections and remote data sources.

**LINQ enables you to query any collection implementing `IEnumerable<T>`, whether an array, list, or XML DOM, as well as remote data sources, such as tables in a SQL Server database.** LINQ offers the benefits of both compile-time type checking and dynamic query composition.

All core types are defined in the `System.Linq` and `System.Linq.Expressions` namespaces.

# Getting Started

The basic units of data in LINQ are sequences and elements. A sequence is any object
that implements `IEnumerable<T>`, and an element is each item in the sequence.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
class LinqDemo
{
 static void Main()
 {
 string[] names = { "Tom", "Dick", "Harry" };
 IEnumerable<string> filteredNames = names.Where (n => n.Length >= 4);
 foreach (string name in filteredNames) Console.WriteLine (name);
 }
}

/*
OUTPUT:
Dick
Harry
*/
```

We could further shorten our code by implicitly typing filteredNames:

```csharp
var filteredNames = names.Where (n => n.Length >= 4);

//fluent syntax
IEnumerable<string> filteredNames = names.Where (n => n.Contains ("a"));

//query expression syntax
IEnumerable<string> filteredNames = from n in names
 where n.Contains ("a")
 select n;
```

# Fluent Syntax

Fluent syntax is the most flexible and fundamental.

```csharp
IEnumerable<string> filteredNames = names.Where (n => n.Contains ("a"));
foreach (string name in filteredNames)
 Console.WriteLine (name); // Harry
```

### Chaining Query Operators

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
            IEnumerable<string> query = names
            .Where(n => n.Contains("a"))
            .OrderBy(n => n.Length)
            .Select(n => n.ToUpper());
            foreach (string name in query) Console.WriteLine(name);
            Console.ReadLine();
            /*
                JAY
                MARY
                HARRY
            */

```

- Where operator, which emits a filtered version of the input sequence.
- The OrderBy operator emits a sorted version of its input sequence
- the Select method emits a sequence in which each input element is transformed or projected with a given lambda expression

```csharp
// The same query constructed progressively:

IEnumerable<string> filtered   = names.Where      (n => n.Contains ("a"));
IEnumerable<string> sorted     = filtered.OrderBy (n => n.Length);
IEnumerable<string> finalQuery = sorted.Select    (n => n.ToUpper());

filtered.Dump   ("Filtered");
sorted.Dump     ("Sorted");
finalQuery.Dump ("FinalQuery");
```

### Why extension methods are important

Instead of using extension method syntax, you can use conventional static method syntax to call the query operators

```csharp
IEnumerable<string> filtered = Enumerable.Where (names,
 n => n.Contains ("a"));
IEnumerable<string> sorted = Enumerable.OrderBy (filtered, n => n.Length);
IEnumerable<string> finalQuery = Enumerable.Select (sorted,
 n => n.ToUpper());
```

This is, in fact, how the compiler translates extension method calls. Shunning extension methods comes at a cost, however, if you want to write a query in a single statement as we did earlier.

```csharp
IEnumerable<string> query = names.Where (n => n.Contains ("a"))
 .OrderBy (n => n.Length)
 .Select (n => n.ToUpper());
```

Its natural linear shape reflects the left-to-right flow of data and keeps lambda expressions alongside their query operators (infix notation). Without extension methods, the query loses its fluency:

```csharp
IEnumerable<string> query =
 Enumerable.Select (
 Enumerable.OrderBy (
 Enumerable.Where (
 names, n => n.Contains ("a")
 ), n => n.Length
 ), n => n.ToUpper()
 );
```

### Composing Lambda Expressions

The purpose of the lambda expression depends on the particular query operator. With the Where operator, it indicates whether an element should be included in the output sequence. In the case of the OrderBy operator, the lambda expression maps each element in the input sequence to its sorting key. With the Select operator, the lambda expression determines how each element in the input sequence is transformed before being fed to the output sequence.

**A lambda expression that takes a value and returns a bool is called a predicate**

A lambda expression in a query operator always works on individual elements in the input sequence not the sequence as a whole

### Natural Ordering

The original ordering of elements within an input sequence is significant in LINQ. Some query operators rely on this ordering, such as `Take`, `Skip`, and `Reverse`.

The Take operator outputs the first x elements, discarding the rest

```csharp
//The Take operator outputs the first x elements, discarding the rest:
int[] numbers = { 10, 9, 8, 7, 6 };
IEnumerable<int> firstThree = numbers.Take (3); // { 10, 9, 8 }

//The Skip operator ignores the first x elements and outputs the rest:
IEnumerable<int> lastTwo = numbers.Skip (3); // { 7, 6 }

//Reverse does exactly as it says:
IEnumerable<int> reversed = numbers.Reverse(); // { 6, 7, 8, 9, 10 }
```

### Other Operators

Not all query operators return a sequence. The element operators extract one element from the input sequence

```csharp
int[] numbers = { 10, 9, 8, 7, 6 };
int firstNumber = numbers.First(); // 10
int lastNumber = numbers.Last(); // 6
int secondNumber = numbers.ElementAt(1); // 9
int secondLowest = numbers.OrderBy(n=>n).Skip(1).First(); // 7
```

```csharp
//The aggregation operators return a scalar value, usually of numeric type:
int count = numbers.Count(); // 5;
int min = numbers.Min(); // 6;

//The quantifiers return a bool value:
bool hasTheNumberNine = numbers.Contains (9); // true
bool hasMoreThanZeroElements = numbers.Any(); // true
bool hasAnOddElement = numbers.Any (n => n % 2 != 0); // true
```

Some query operators accept two input sequences. Examples are Concat, which appends one sequence to another, and Union, which does the same but with duplicates removed

```csharp
int[] seq1 = { 1, 2, 3 };
int[] seq2 = { 3, 4, 5 };
IEnumerable<int> concat = seq1.Concat (seq2); // { 1, 2, 3, 3, 4, 5 }
IEnumerable<int> union = seq1.Union (seq2); // { 1, 2, 3, 4, 5 }
```

# Query Expressions

C# provides a syntactic shortcut for writing LINQ queries, called query expressions.

Contrary to popular belief, a query expression is not a means of embedding SQL into C#. In fact, the design of query expressions was inspired primarily by list comprehensions from functional programming languages such as LISP and Haskell, although SQL had a cosmetic influence.

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };

IEnumerable<string> query =
    from     n in names
    where    n.Contains ("a")   // Filter elements
    orderby  n.Length           // Sort elements
    select   n.ToUpper();       // Translate each element (project)

foreach (string name in query) Console.WriteLine (name);
/*
                JAY
                MARY
                HARRY
            */
```

Query expressions always start with a from clause and end with either a select or a group clause. The from clause declares a range variable (in this case, n), which you can think of as traversing the input sequence rather like foreach.

The compiler processes a query expression by translating it into fluent syntax. It does this in a fairly mechanical fashion—much like it translates foreach statements into calls to GetEnumerator and MoveNext. This means that anything you can write in query syntax you can also write in fluent syntax. The compiler (initially) translates our example query into the following:

```csharp
IEnumerable<string> query = names.Where (n => n.Contains ("a"))
 .OrderBy (n => n.Length)
 .Select (n => n.ToUpper());
```

### Range Variables

The identifier immediately following the from keyword syntax is called the range variable. A range variable refers to the current element in the sequence on which the operation is to be performed.

In our examples, the range variable n appears in every clause in the query. And yet, the variable actually enumerates over a different sequence with each clause:

```csharp
from n in names // n is our range variable
where n.Contains ("a") // n = directly from the array
orderby n.Length // n = subsequent to being filtered
select n.ToUpper() // n = subsequent to being sorted
```

This becomes clear when we examine the compiler’s mechanical translation to fluent syntax:

```csharp
names.Where (n => n.Contains ("a")) // Locally scoped n
 .OrderBy (n => n.Length) // Locally scoped n
 .Select (n => n.ToUpper()) // Locally scoped n
```

### Mixed-Syntax Queries

If a query operator has no query-syntax support, you can mix query syntax and fluent syntax. The only restriction is that each query-syntax component must be complete

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };

(from n in names where n.Contains ("a") select n).Count()
    .Dump ("Names containing the letter 'a'");

string first = (from n in names orderby n select n).First()
    .Dump ("First name, alphabetically");

names.Where (n => n.Contains ("a")).Count()
    .Dump ("Original query, entirely in fluent syntax");
    
names.OrderBy (n => n).First()
    .Dump ("Second query, entirely in fluent syntax");
```

# Deferred Execution

An important feature of most query operators is that they execute not when constructed, but when enumerated (in other words, when MoveNext is called on its enumerator).

```csharp
var numbers = new List<int> { 1 };
IEnumerable<int> query = numbers.Select (n => n * 10); // Build query
numbers.Add (2); // Sneak in an extra element
foreach (int n in query)
Console.Write (n + "|"); // 10|20|
```

The extra number that we sneaked into the list after constructing the query is included in the result because it’s not until the foreach statement runs that any filtering or sorting takes place. This is called deferred or lazy execution and is the same as what happens with delegates.

All standard query operators provide deferred execution, with the following exceptions

- Operators that return a single element or scalar value, such as First or Count
- The following conversion operators:ToArray, ToList, ToDictionary, ToLookup, ToHashSet

These operators cause immediate query execution because their result types have no mechanism to provide deferred execution. The Count method, for instance, returns a simple integer, which doesn’t then get enumerated. The following query is executed immediately:

```csharp
int matches = numbers.Where (n => n <= 2).Count(); // 1
```

Deferred execution is important because it decouples query construction from query execution. This allows you to construct a query in several steps, and makes database queries possible.

### Reevaluation

Deferred execution has another consequence: a deferred execution query is reevaluated when you reenumerate:

```csharp
var numbers = new List<int>() { 1, 2 };
IEnumerable<int> query = numbers.Select (n => n * 10);
foreach (int n in query) Console.Write (n + "|"); // 10|20|
numbers.Clear();
foreach (int n in query) Console.Write (n + "|"); // <nothing>
```

There are a couple of reasons why reevaluation is sometimes disadvantageous

- Sometimes, you want to “freeze” or cache the results at a certain point in time.
- Some queries are computationally intensive (or rely on querying a remote data‐
base), so you don’t want to unnecessarily repeat them

You can defeat reevaluation by calling a conversion operator such as ToArray or ToList.

```csharp
var numbers = new List<int>() { 1, 2 };
List<int> timesTen = numbers
 .Select (n => n * 10)
 .ToList(); // Executes immediately into a List<int>
numbers.Clear();
Console.WriteLine (timesTen.Count); // Still 2
```

### Captured Variables

If your query’s lambda expressions capture outer variables, the query will honor the
value of those variables at the time the query runs:

```csharp
int[] numbers = { 1, 2 };
int factor = 10;
IEnumerable<int> query = numbers.Select (n => n * factor);
factor = 20;
foreach (int n in query) Console.Write (n + "|"); // 20|40|
```

This can be a trap when building up a query within a for loop. F

```csharp
IEnumerable<char> query = "Not what you might expect";
query = query.Where (c => c != 'a');
query = query.Where (c => c != 'e');
query = query.Where (c => c != 'i');
query = query.Where (c => c != 'o');
query = query.Where (c => c != 'u');
foreach (char c in query) Console.Write (c); // Nt wht y mght xpct

IEnumerable<char> query = "Not what you might expect";
string vowels = "aeiou";
for (int i = 0; i < vowels.Length; i++)
 query = query.Where (c => c != vowels[i]);// IndexOutOfRangeException
foreach (char c in query) Console.Write (c);
// An IndexOutOfRangeException is thrown! This is because, as we saw in Chapter 4 
// (see "Capturing Outer Variables"), the compiler scopes the iteration variable 
// in the for loop as if it was declared outside the loop. Hence each closure captures
// the same variable (i) whose value is 5 when the query is enumerated.
```

To solve this, you must assign the loop variable to another variable declared inside the statement block

```csharp
for (int i = 0; i < vowels.Length; i++)
{
 char vowel = vowels[i];
 query = query.Where (c => c != vowel);
}
```

# Subqueries

A subquery is a query contained within another query’s lambda expression. The following example uses a subquery to sort musicians by their last name:

```csharp
string[] musos =
 { "David Gilmour", "Roger Waters", "Rick Wright", "Nick Mason" };
IEnumerable<string> query = musos.OrderBy (m => m.Split().Last());
```

`m.Split` converts each string into a collection of words, upon which we then call the Last query operator. `m.Split().Last` is the subquery

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> outerQuery = names
 .Where (n => n.Length == names.OrderBy (n2 => n2.Length)
 .Select (n2 => n2.Length).First());
// Tom, Jay
```

Here’s the same thing as a query expression:

```csharp
IEnumerable<string> outerQuery =
 from n in names
 where n.Length ==
 (from n2 in names orderby n2.Length select n2.Length).First()
 select n;
```

Because the outer range variable (n) is in scope for a subquery, we cannot reuse n as the subquery’s range variable.
A subquery is executed whenever the enclosing lambda expression is evaluated. This means that a subquery is executed upon demand, at the discretion of the outer query. You could say that execution proceeds from the outside in. Local queries follow this model literally; interpreted queries (e.g., database queries) follow this model conceptually.

We can express our preceding subquery more succinctly as follows:

```csharp
IEnumerable<string> query =
 from n in names
 where n.Length == names.OrderBy (n2 => n2.Length).First().Length
 select n;
```

With the Min aggregation function, we can simplify the query further

```csharp
IEnumerable<string> query =
 from n in names
 where n.Length == names.Min (n2 => n2.Length)
 select n;
```

we describe how remote sources such as SQL tables can be queried. Our example makes an ideal database query because it would be processed as a unit, requiring only one round trip to the database server. This query, however, is inefficient for a local collection because the subquery is recalculated on each outer loop iteration. We can avoid this inefficiency by running the subquery separately (so that it’s no longer a subquery):

```csharp
int shortest = names.Min (n => n.Length);
IEnumerable<string> query = from n in names
 where n.Length == shortest
 select n;
```

# Composition Strategies

In this section, we describe three strategies for building more complex queries

- Progressive query construction
- Using the into keyword
- Wrapping queries

### Progressive Query Building

At the start of the chapter, we demonstrated how you could build a fluent query progressively

```csharp
var filtered = names .Where (n => n.Contains ("a"));
var sorted = filtered .OrderBy (n => n);
var query = sorted .Select (n => n.ToUpper());
```

Because each of the participating query operators returns a decorator sequence, the resultant query is the same chain or layering of decorators that you would get from a single-expression query. There are a couple of potential benefits, however, to building queries progressively

- It can make queries easier to write.
- You can add query operators conditionally.

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query = names
 .Select (n => n.Replace ("a", "").Replace ("e", "").Replace ("i", "")
 .Replace ("o", "").Replace ("u", ""))
 .Where (n => n.Length > 2)
 .OrderBy (n => n);
// Dck
// Hrry
// Mry
```

Translating this directly into a query expression is troublesome because the select clause must come after the where and orderby clauses. And if we rearrange the query so as to project last, the result would be different:

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query =
 from n in names
 where n.Length > 2
 orderby n
 select n.Replace ("a", "").Replace ("e", "").Replace ("i", "")
 .Replace ("o", "").Replace ("u", "");
// Dck
// Hrry
// Jy
// Mry
// Tm
```

Fortunately, there are a number of ways to get the original result in query syntax

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query =
 from n in names
 select n.Replace ("a", "").Replace ("e", "").Replace ("i", "")
 .Replace ("o", "").Replace ("u", "");
query = from n in query where n.Length > 2 orderby n select n;
// Dck
// Hrry
// Mry
```

### The into Keyword

The into keyword is interpreted in two very different ways by query expressions, depending on context. The meaning we’re describing now is for signaling query continuation (the other is
for signaling a GroupJoin). 

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query =
 from n in names
 select n.Replace ("a", "").Replace ("e", "").Replace ("i", "")
 .Replace ("o", "").Replace ("u", "")
 **into noVowel**
 where noVowel.Length > 2 orderby noVowel select noVowel;
```

**The only place you can use into is after a select or group clause. into restarts a query, allowing you to introduce fresh where, orderby, and select clauses.**

Although it’s easiest to think of into as restarting a query from the perspective of a query expression, it’s all one query when translated to its final fluent form. Hence, there’s no
intrinsic performance hit with into. Nor do you lose any points for its use!

**All range variables are out of scope following an into keyword. The following will not compile:**

```csharp
var query =
 from n1 in names
 select n1.ToUpper()
 into n2 // Only n2 is visible from here on.
 where n1.Contains ("x") // Illegal: n1 is not in scope.
 select n2;
```

### Wrapping Queries

A query built progressively can be formulated into a single statement by wrapping one query around another. In general terms

```csharp
var tempQuery = tempQueryExpr
var finalQuery = from ... in tempQuery ...
//can be reformulated as:
var finalQuery = from ... in (tempQueryExpr)
```

```csharp
var names = new[] { "Tom", "Dick", "Harry", "Mary", "Jay" }.AsQueryable();

IEnumerable<string> query =
    from n1 in 
    (
        from   n2 in names
        select n2.Replace ("a", "").Replace ("e", "").Replace ("i", "").Replace ("o", "").Replace ("u", "")
    )
    where n1.Length > 2 orderby n1 select n1;

```

# Projection Strategies

### Object Initializers

```csharp
void Main()
{
    var names = new[] { "Tom", "Dick", "Harry", "Mary", "Jay" }.AsQueryable();

    IEnumerable<TempProjectionItem> temp =
        from n in names
        select new TempProjectionItem
        {
            Original  = n,
            Vowelless = n.Replace ("a", "").Replace ("e", "").Replace ("i", "").Replace ("o", "").Replace ("u", "")
        };
    
    temp.Dump();
}

class TempProjectionItem
{
    public string Original;      // Original name
    public string Vowelless;     // Vowel-stripped name
}
// Dick
// Harry
// Mary
```

### Anonymous Types

Anonymous types allow you to structure your intermediate results without writing special classes.

```csharp
var names = new[] { "Tom", "Dick", "Harry", "Mary", "Jay" }.AsQueryable();

var intermediate = from n in names
select new
{
    Original = n,
    Vowelless = n.Replace ("a", "").Replace ("e", "").Replace ("i", "").Replace ("o", "").Replace ("u", "")
};

(
    from    item in intermediate
    where   item.Vowelless.Length > 2
    select  item.Original
)
.Dump();

// With the into keyword we can do this in one step:

(
    from n in names
    select new
    {
        Original = n,
        Vowelless = n.Replace ("a", "").Replace ("e", "").Replace ("i", "").Replace ("o", "").Replace ("u", "")
    }
    into   temp
    where  temp.Vowelless.Length > 2
    select temp.Original
)
.Dump ("With the 'into' keyword");
```

### The let Keyword

The let keyword introduces a new variable alongside the range variable

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query =
 from n in names
 let vowelless = n.Replace ("a", "").Replace ("e", "").Replace ("i", "")
 .Replace ("o", "").Replace ("u", "")
 where vowelless.Length > 2
 orderby vowelless
 select n; // Thanks to let, n is still in scope.
```

let accomplishes two things:

- It projects new elements alongside existing elements.
- It allows an expression to be used repeatedly in a query without being rewritten.# Chapter 9 : LINQ Operators

<aside>
🚫 This Chapter Currently Not Applicable to Current Work So I am Leaving it for Future Re Read

</aside>

# Overview

This chapter describes each of the LINQ query operators. This Chapter cover area such as

- Projecting object hierarchies
- Joining with Select, SelectMany, Join, and GroupJoin
- Query expressions with multiple range variables

All of the examples in this chapter assume that a names array is defined as follows

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
```

Examples that query a database assume that a variable called dbContext is instanti‐
ated as

```csharp
var dbContext = new NutshellContext();
```

- where NutshellContext is defined as follows
    
    ```csharp
    public class NutshellContext : DbContext
    {
     public DbSet<Customer> Customers { get; set; }
     public DbSet<Purchase> Purchases { get; set; }
     protected override void OnModelCreating(ModelBuilder modelBuilder)
     {
     modelBuilder.Entity<Customer>(entity =>
     {
     entity.ToTable("Customer");
     entity.Property(e => e.Name).IsRequired(); // Column is not nullable
     });
     modelBuilder.Entity<Purchase>(entity =>
     {
     entity.ToTable("Purchase");
     entity.Property(e => e.Date).IsRequired();
    entity.Property(e => e.Description).IsRequired();
     });
     }
    }
    public class Customer
    {
     public int ID { get; set; }
     public string Name { get; set; }
     public virtual List<Purchase> Purchases { get; set; }
     = new List<Purchase>();
    }
    public class Purchase
    {
     public int ID { get; set; }
     public int? CustomerID { get; set; }
     public DateTime Date { get; set; }
     public string Description { get; set; }
     public decimal Price { get; set; }
     public virtual Customer Customer { get; set; }
    }
    ```
    
- Here are corresponding SQL Server table definitions
    
    ```csharp
    CREATE TABLE Customer (
     ID int NOT NULL IDENTITY PRIMARY KEY,
     Name nvarchar(30) NOT NULL
    )
    CREATE TABLE Purchase (
     ID int NOT NULL IDENTITY PRIMARY KEY,
     CustomerID int NOT NULL REFERENCES Customer(ID),
     Date datetime NOT NULL,
     Description nvarchar(30) NOT NULL,
     Price decimal NOT NULL
    )
    ```
    

In this section, we provide an overview of the standard query operators. They fall
into three categories:

- Sequence in, sequence out (sequence→sequence)
- Sequence in, single element or scalar value out
- Nothing in, sequence out (generation methods)

## Sequence→Sequence

Most query operators fall into this category—accepting one or more sequences as
input and emitting a single output sequence

### Filtering

IEnumerable<TSource>→IEnumerable<TSource>
Returns a subset of the original elements.
Where, Take, TakeWhile, Skip, SkipWhile, Distinct

### Projecting

Transforms each element with a lambda function. SelectMany flattens nested
sequences; Select and SelectMany perform inner joins, left outer joins, cross joins,
and non-equi joins with EF Core.

Select, SelectMany

### Joining

Meshes elements of one sequence with another. Join and GroupJoin operators are
designed to be efficient with local queries and support inner and left outer joins

The Zip operator enumerates two sequences in step, applying a function over each

element pair. Rather than naming the type arguments TOuter and TInner, the Zip
operator names them TFirst and TSecond:

IEnumerable

, IEnumerable→IEnumerable
Join, GroupJoin, Zip
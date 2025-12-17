---
title: The Markdown Guide
---

# The Markdown Guide by Matt Cone

## Markdown Basic

### Comments

- For Single Line Comments

```markdown
<!--CommentsHere-->
```

- For Multi Line Comments : ==Typora Don’t Support This==

```markdown
<!--
Line1
Line2
-->
```

### Video

You can use the `<video>` HTML tag to embed videos. For example:

```markdown
<video src="xxx.mp4" />
```

### Embed Contents

Some websites provide iframe-based embed code which you can also paste into Typora. For example:

```markdown
<iframe height='265' scrolling='no' title='Fancy Animated SVG Menu' src='<http://codepen.io/jeangontijo/embed/OxVywj/?height=265&theme-id=0&default-tab=css,result&embed-version=2>' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'></iframe>
```

---

## Markdown Blockquote with HTML Tags

```markdown
> **&#9888; Note: **
> 
> cite="http://www.worldwildlife.org/who/index.html">
> For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.
```

### Blockquote with HTML Tag

```markdown
<blockquote style="background:#6A5ACD ;color:white"> 😜
</br> This is Test Note.</br> This the first paragraph.
And this is the second paragraph.
</blockquote>

<blockquote style="background:#FFF380 ;color:Black">
<b>⚠ This is Test Note.</b>
This the first paragraph.
And this is the second paragraph.
</blockquote>

<blockquote style="background:#FDE9D9 ;color:black">
<b>⚠ This is Test Note.</b></br></br>
cite="[http://www.worldwildlife.org/who/index.html](http://www.worldwildlife.org/who/index.html)">
For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.

<blockquote style="background:#F44747 ;color:white">
<b>⚠ This is Test Note.</b>
This the first paragraph.
And this is the second paragraph.
</blockquote>

<blockquote style="background-color:#6A5ACD;color:white" >
⚠ Note :</br>
cite="[http://www.worldwildlife.org/who/index.html](http://www.worldwildlife.org/who/index.html)">
For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.
</blockquote>
```

---

## Markdown Code Block

### Visual Basic

```visual-basic
Private Sub Form_Load()
	' Execute a simple message box that says "Hello, World!"
	MsgBox "Hello, World!"
End Sub  
```

### C#

```csharp
namespace CSharpTutorials
{
   class Program
   {
	   static void Main(string[] args)
	   {
		   string message = "Hello World!!";

		   Console.WriteLine(message);
	   }
   }
}
```

---

## MarkDown Flowchart

```
pie title NETFLIX
		 "Time spent looking for movie" : 90
		 "Time spent watching it" : 10

```

```
  graph LR
  Markdown --> HTML --> RenderedDocument

```

```
graph TD;
	A-->B;
	A-->C;
	B-->D;
	C-->D;

```

```
graph TD
A[Client] --> B[Load Balancer]
B --> C[Server01]
B --> D[Server02]

```

```
classDiagram
	  Animal <|-- Duck
	  Animal <|-- Fish
	  Animal <|-- Zebra
	  Animal : +int age
	  Animal : +String gender
	  Animal: +isMammal()
	  Animal: +mate()
	  class Duck{
		  +String beakColor
		  +swim()
		  +quack()
	  }
	  class Fish{
		  -int sizeInFeet
		  -canEat()
	  }
	  class Zebra{
		  +bool is_wild
		  +run()
	  }

```

---

## Markdown Highlight text with HTML Tags

### Highlight Text Color (White Fonts)

```csharp
**<span style="color:#F44747">Test</Span>**
<b><mark style="background-color: #F44747 ;color:white">This is Highlighted Text</mark></b>

**<span style="color:SlateBlue">Test</Span>**
<b><mark style="background-color: SlateBlue ;color:white">This is Highlighted Text</mark></b>

**<span style="color:DodgerBlue">Test</Span>**
<b><mark style="background-color: DodgerBlue ;color:white">This is Highlighted Text</mark></b>

**<span style="color:MediumSeaGreen">Test</Span>**
<b><mark style="background-color: MediumSeaGreen ;color:white">This is Highlighted Text</mark></b>
```

### Highlight Text Color (Black Fonts)

```csharp
<b><mark style="background-color:	rgb(255,255,51,0.7) ">This is Highlighted Text</mark></b>

<b><mark style="background-color: #FFF380">This is Highlighted Text</mark></b>

<b><mark style="background-color: #A2E4FA ;color:black">This is Highlighted Text</mark></b>

<b><mark style="background-color: #93F380">This is Highlighted Text</mark></b>

<b><mark style="background-color: #FDE9D9">This is Highlighted Text</mark></b>
```


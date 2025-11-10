# Typography 
This is sample Header 1

## Header 2
This is sample Header 2

### Header 3 
This is sample Header 3


## Page Break
Next Section will start on new page in pdf
<div style="page-break-after: always; break-after: page;"></div>

## Paragraphs
This is a simple paragraph to test text formatting. 

Another paragraph with **bold**, *italic*, and ~~strikethrough~~ text. Also, some `inline code`.

## Bullet List
- This is bullet list 1
- This is bullet list 2
- This is bullet list 3
  - Nested bullet 1
  - Nested bullet 2

## Numbered List
1. Numbered List 1
2. Numbered List 2
3. Numbered List 3
   1. Nested Numbered 1
   2. Nested Numbered 2

## Checklist
- [x] Checked item
- [ ] Unchecked item
- [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
- [ ] Vestibulum convallis sit amet nisi a tincidunt
    * [x] In hac habitasse platea dictumst
    * [x] In scelerisque nibh non dolor mollis congue sed et metus
    * [ ] Praesent sed risus massa
- [ ] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque

## Definition List
Term 1
: Definition for term 1

Term 2
: Definition for term 2

## Emojis
- ✅ Ok
- ❌ Error
- ⚠️ Warning
- ℹ️ Info


## Code Block

### Regular
```python
# This is a code block
print("Hello, World!")
```

### With Title
``` py title="bubble_sort.py"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

### Line Numbers
``` py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

### Highlight specific lines
``` py hl_lines="2 3"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

## Links
- [GitHub](https://github.com)
- [Youtube](https://github.com)


## Blockquote
> This is a blockquote. It can span multiple lines.
> Second line of blockquote.

## Call outs

!!! tip

Tip/Idea callout

!!! info

Info Type Callout

!!! warning 

warning Type Callout

!!! danger

danger Type Callout

!!! note

note callout
 
!!! quote

quote callout

## Math
Inline math: $E = mc^2$

Block math:
$$
\int_{a}^{b} x^2 dx
$$

## Footnote
Here is a statement with a footnote.[^1]

[^1]: This is the footnote text.

## Table

### Normal
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1    | Data 1   | Data 2   |
| Row 2    | Data 3   | Data 4   |

| Class   | Small screen size `font-size`  | Large screen size `font-size` |
|:--------|:-------------------------------|:------------------------------|
| `.fs-1` | 9px                            | 10px                          |
| `.fs-2` | 11px                           | 12px                          |
| `.fs-3` | 12px                           | 14px                          |
| `.fs-4` | 14px                           | 16px                          |
| `.fs-5` | 16px                           | 18px                          |
| `.fs-6` | 18px                           | 24px                          |
| `.fs-7` | 24px                           | 32px                          |
| `.fs-8` | 32px                           | 38px                          |
| `.fs-9` | 38px                           | 42px                          |
| `.fs-10`| 42px                           | 48px                          |

### Table with Math

| **Part** | **Formula** | **Calculation** | **Area (mm²)** |
| :---: | :---: | :---: | :---: |
| Top Flange | $bf_1 \times tf_1$ | $130 \times 10$ | 1300 |
| Web | $dw_1 \times tw_1$ | $1180 \times 8$ | 9440 |
| Bottom Flange | $bf_1 \times tf_1$ | $130 \times 10$ | 1300 |
| **Total Area** |   |   | **12040** |


## Horizontal Separator

---

## Image 

### Local
![Nodes Logo](assets/favicon.png)

### Online
![Sample Image](https://nodesautomations.com/assets/images/home.webp)

## Diagrams
``` mermaid
graph LR
  A[Start] --> B{Error?};
  B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```
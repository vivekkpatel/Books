# Theme

# KaTeX

[KaTeX] is a lightweight library that focuses on speed and simplicity. It
supports a subset of LaTeX syntax and can render math to HTML and SVG. To use
[KaTeX] within your project, add the following lines to your `mkdocs.yml`.

Location :  `docs/javascripts/katex.js`"
``` js
document$.subscribe(({ body }) => { // (1)!
    renderMathInElement(body, {
    delimiters: [
        { left: "$$",  right: "$$",  display: true },
        { left: "$",   right: "$",   display: false },
        { left: "\\(", right: "\\)", display: false },
        { left: "\\[", right: "\\]", display: true }
    ],
    })
})
```

# Callouts
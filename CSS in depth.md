### Notes from CSS in depth from Frontendmaster

#### @media print

Use CSS selectors to improve printing user experience.

```css
@media print{
  abbr[title]:after {
    content: "(" attr(title) ")";
  }
  a[href^=http]:after {
    content: "(" attr(href) ")";
  }
  a[href$=pdf]:after { 
    content: " (PDF)"; 
  }
}
```

#### Form related UI pseudo-classes

- input:valid { border: 1px solid green; }
- input:invalid { border: 1px solid red; }
- input:required, input[aria-required="true"] { border-width: 5px; }
- input:optional { border-width: 10px; }
- input:out-of-range { background-color: pink; }
- input:in-range { background-color:lightgreen; }

#### :root

Selects the document root, which is <html>

- Declare font-size on :root if using rem units
- Define variables on root


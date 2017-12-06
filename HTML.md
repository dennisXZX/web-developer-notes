## HTML

#### HTML attribute vs DOM property

HTML is a set of written instructions for how to display a web page. The browser reads the HTML and creates something called a DOM (Document Object Model) in memory. Changing the HTML doesn’t automatically update the webpage unless the user refreshes the browser, changing the DOM however instantly updates the webpage. 

There is mostly a 1-to-1 mapping between the names and values of HTML attributes and their equivalent DOM properties, but not always. The hidden HTML attribute is a good example, it only needs to exist on an HTML element to instruct the browser to hide the element. So `hidden="true"` hides the element but confusingly so does `hidden="false"`, in HTML we just need to add hidden to hide the element.

```
<div hidden>I'm hiding</div>
```

The DOM representation of the hidden attribute is a property also called `hidden`, which if set to true hides the element and false shows the element.

Javascript framework like Angular doesn’t manipulate HTML attributes, it manipulates DOM properties because the DOM is what actually gets displayed. So when we write `[hidden]` we are manipulating the DOM property and not the HTML attribute.

#### Why we need to use HTML entities?

Since some characters are reserved in HTML, such as `<` (`&lt;`), `>` (`&gt;`) and `&` (`&amp;`), so we need to escape them for the HTML code to be parsed correctly. Mordern browsers are lenient on parsing but it is a good pratice to escape all these little demons.

#### What does a `doctype` do?

DOCTYPEs are required for legacy reasons. When omitted, browsers tend to use a different rendering mode that is incompatible with some specifications. Including the DOCTYPE in a document ensures that the browser makes a best-effort attempt at following the relevant specifications.

#### How to make a website accessible?

Here are a few important things that need to be considered in making a website accessible.

- Use semantic HTML to structure your website
- Always include alt text for images
- Give links descriptive names
- Ensure that all content can be accessed by keyboard
- Use ARIA roles to enhance the semantics of your website

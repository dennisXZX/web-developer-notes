## HTML

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

#### What is microformats?

Microformats is a way to extend HTML markup by introducing some standardized class names for information they contain. Below is a hCard 

```
<p class="h-card">
  <img class="u-photo" src="http://example.org/photo.png" alt="" />
  <a class="p-name u-url" href="http://example.org">Joe Bloggs</a>
  <a class="u-email" href="mailto:joebloggs@example.com">joebloggs@example.com</a>, 
  <span class="p-street-address">17 Austerstræti</span>
  <span class="p-locality">Reykjavík</span>
  <span class="p-country-name">Iceland</span>
</p>
```

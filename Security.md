## Security

### Cross-site scripting (XSS)

An XSS attack exploits a weakness in a page that include a variable submitted in a request to show up in raw form in the response. The page is only reflecting back what was submitted in that request, but the content of that request might hold characters that break out of ordinary text content and introduce HTML or javascript content that the developer did not intend.

For example, you can inject a `<script>malicious code</script>` tag into an input form and submit it to a server, if the server does not handle XSS attack properly, it is screwed up.

### Cross Site Request Forgery (CSRF)

### SQL injection

### Man in the middle

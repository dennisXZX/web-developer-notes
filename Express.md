## Express (Node.js framework)

#### Set up a simple server

```js
const jsonData = {
    count: 12,
    message: 'hey'
};

const express = require('express');
const app = express();

const fs = require('fs');

app.get('/', (req, res) => {
    // sendFile() takes an absolute path to a file and
    // sets the mime type based on the file extension name
    // under the hood it uses the built-in 'fs' NodeJS module
   res.sendFile(__dirname + '/index.html', (err) => {
       if (err) {
           // set the status code
           res.status(500).send(err);
       }
   });

    // use 'fs' module to serve the file
    fs.readFile('index.html', (err, buffer) => {
        const html = buffer.toString();

        res.setHeader('Content-Type', 'text/html');
        res.send(html);
    })
});

// res.send() converts to JSON as well
// but req.json() will convert things like null and undefined to JSON too
app.get('/data', (req, res) => res.json(jsonData));

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```

For better dev experiences, install `nodemon (npm i -g nodemon)` and use `nodemon server.js` to launch the server.

#### RESTful services

Imagine we have to design an API that deals with a list of customers. Normally we need to think about the `CRUD` (Create, Read, Update and Delete) operations.

- GET /api/customers, server responds with a list of customers
- GET /api/customers?sort=name, server responds with a list of customers sorted by name
- GET /api/customers/1, server responds with a customer with the given id
- PUT /api/customers/1, server updates the customer with the given id using the data sent in the request body
- DELETE /api/customers/1, server deletes the customer with the given id
- POST /api/customers, server creates a new customer

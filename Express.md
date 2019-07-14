## Express (Node.js framework)

#### Server side rendering

`res.render()` is for server-side rendering, which means the server generates HTML, CSS and Javascript (mostly through a template engine) and sends back to browser via Apache or Nginx. 

You can access `res.locals` in the template views (EJS, handlebars or Pug), which means you can put stuff in `res.locals` through middlewares, and those stuff can be accessed in the template views. For example, you can add `res.locals.validated` in a validation middleware, then you can use the variable in the EJS template like `<%= validated %>`.

```js
app.set('view engine', 'ejs')
app.uset('views', path.join(__dirname, 'views'))
```

#### Middlewares

Express is a framework composed of middlewares. A middleware is a function that has access to `req`, `res` and `next`.

```js
// add secure headers
app.use(helmet())

// handle static files
app.use(express.static())

// parse JSON data from POST or PUT requests
app.use(express.json())

// parse urlencoded data from POST or PUT requests with "application/x-www-form-urlencoded" content type
app.use(express.urlencoded())

// even a routing in Express is just a middleware that never calls next()
app.get('/', (req, res) => {
    code here...
})
```

#### Set up a simple server

```js
const jsonData = {
    count: 12,
    message: 'hey'
};

const express = require('express');
const app = express();

const fs = require('fs');

// serving a file using GET request
app.get('/', (req, res) => {
    // sendFile() takes an absolute path to a file and
    // sets the mime type based on the file extension name
    // under the hood it uses the built-in 'fs' Node module
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

// serving JSON data using GET request
// res.send() converts to JSON as well
// but req.json() will convert things like null and undefined to JSON too
app.get('/data', (req, res) => res.json(jsonData));

// retrieving all the courses using GET request
// retrieve route params (req.params.id) and queries (req.query.sortBy)
// when user hits http://domain/api/courses/1?sortBy=name
app.get('/api/courses/:id', (req, res) => {
    // find the course with the given id
    const course = courses.find(c => c.id === parseInt(req.params.id))
    if (!course) {
        res.status(404).send('The course wit the given ID is not found')
        return
    }
    
    // return the course if it's found
    res.send(course)
})

// update a course using PUT request
app.put('api/courses/:id', (req, res) => {
    // find the course with the given id
    const course = courses.find(c => c.id === parseInt(req.params.id))
    if (!course) {
        res.status(404).send('The course wit the given ID is not found')
        return
    }
    
    // using joi library to validate
    const { error } = validateCourse(req.body)
    
    if (error) {
        res.status(400).send(error.details[0].message)
        return
    }
    
    course.name = req.body.name
    res.send(course)
})

// delete a course using DELETE request
app.delete('api/course/:id'. (req, res) => {
    // find the course with the given id
    const course = courses.find(c => c.id === parseInt(req.params.id))
    if (!course) {
        res.status(404).send('The course wit the given ID is not found')
        return
    }   
    
    // delete the course
    const index = courses.indexOf(course)
    courses.splice(index, 1)
    
    res.send(course)
})

// helper function for validation
function validateCourse(course) {
    const schema = {
        name: Joi.string().min(3).required()
    }
    
    return Joi.validate(course, schema)
}

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Example app listening on port ${PORT}!`));
```

For better dev experiences, install `nodemon (npm i -g nodemon)` and use `nodemon server.js` to launch the server.

#### Apply middlewares based on different environments

Use `app.get('env')` to check the current environment, under the hood it retrieve the value of `process.env.NODE_ENV`.

```js
if (app.get('env') === 'development') {
    app.use(morgan('tiny'));
    console.log('Morgan enabled...');
}
```

#### Config setting

Use [node-config](https://github.com/lorenwest/node-config) to manage your config.

#### Custom middleware

A middleware in Express is just a function with a specific signature.

```js
const logger = (req, res, next) => {
    /* code for logging */
    
    // handle the responsibility to the next middleware
    next();
}

module.exports = logger;
```

Use middleware

```js
const logger = require('./middleware/logger');

// use logger middleware to all routes
app.use(logger);
// user logger middleware to a specific route
app.use(‘/api/admin’, logger);
```

#### RESTful services

Imagine we have to design an API that deals with a list of customers. Normally we need to think about the `CRUD` (Create, Read, Update and Delete) operations.

- GET /api/customers, server responds with a list of customers
- GET /api/customers?sort=name, server responds with a list of customers sorted by name
- GET /api/customers/1, server responds with a customer with the given id
- PUT /api/customers/1, server updates the customer with the given id using the data sent in the request body
- DELETE /api/customers/1, server deletes the customer with the given id
- POST /api/customers, server creates a new customer

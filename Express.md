## Express (Node.js framework)

#### RESTful services

Imagine we have to design an API that deals with a list of customers. Normally we need to think about the `CRUD` (Create, Read, Update and Delete) operations.

- GET /api/customers, server responds with a list of customers
- GET /api/customers/1, server responds with a customer with the given id
- PUT /api/customers/1, server updates the customer with the given id using the data sent in the request body
- DELETE /api/customers/1, server deletes the customer with the given id
- POST /api/customers, server creates a new customer

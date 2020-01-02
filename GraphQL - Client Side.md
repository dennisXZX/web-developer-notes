### GraphQL

GraphQL is a query language for your API. It provides clients the power to ask for exactly what they need and nothing more. Additionally, you can get all you want in one request.

#### Query

```
# 'query viewerInfo' is an operation name, just like a function name
# $isOwner is a variable passed into the query
query viewerInfo($isOwner: Boolean!) {
  viewer {
    login
    bio
    id
    name
    starredRepositories(ownedByViewer: $isOwner, last: 5) {
      nodes {
        id
        name
      }
    }
    firstFollowers: followers(first: 3) {
      nodes {
        ...userInfo
      }
    }
    lastFollowers: followers(last: 5) {
      nodes {
        ...userInfo
      }
    }
  }
}

# Fragment is like function in programming language
fragment userInfo on User {
  id
  bio
  bioHTML
  avatarUrl
}
```

#### Mutation

```
mutation NewStatus($input: ChangeUserStatusInput!) {
  changeUserStatus(input: $input) {
    clientMutationId
    status {
      message
    }
  }
}
```

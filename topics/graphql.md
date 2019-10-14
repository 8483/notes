GraphQL is an alternative to REST API. It's a typed query language.

It allows flexible querying from the front-end i.e. client.

It solves the problem of needing **just** a piece of data, without getting all the data from an end-point.  

REST API would handle this for a `GET /user` request with:
- `GET /user-slim` - Lots of routes and updating.
- `GET /user?data=slim` - The API becomes complex

GraphQL would handle this with `POST /user` by sending this query expression in the body.

```js
{
    query {           // operation type
        user {        // operation end-point
            name      // requested field
            age
        }
    }
}
```

GraphQL **always** uses `POST` because it sends the query expression.

| REST API | GraphQL |
| ------------- |-------------|
| GET | `query` operation type |
| POST, PUT, PATCH, DELETE | `mutation` operation type |
| Routes | Query definitions |
| Controllers | Resolvers |
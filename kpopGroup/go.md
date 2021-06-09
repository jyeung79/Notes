## Protocol Buffers

It seems to be another format. Not `XML` or `JSON`.
* Stores structured data that can be serialized or de-serialized
* A lot smaller than XML & JSON

### Example user data

Represented in XML
```XML
<person>
  <name>Elliot</name>
  <age>24</age>
</person>
```
Represented in JSON
```JSON
{
  "name": "Elliot",
  "age": 24
}
```
Protocol Buffer
```proto3
[10 6 69 108 108 105 111 116 16 24]
```
## GraphQL Implementation

gqlgen seems to be suffering from [batch loading issues](https://github.com/99designs/gqlgen/issues/518)
- A single goroutine for each resolver
  - As of this moment, I don't know what that means

graphql-go seems to have too much boilerplate to do things (thunkgs) to batch calls and such. Also there's [issues with circular dependancies.](https://www.reddit.com/r/golang/comments/9udwve/introducing_gqlgen_a_graphql_server_generator_for/) 

gqlgen seems to be easier to use but seems to result in higher cpu usage. Perhaps it comes from the fact that there's a single goroutine (a thread) that runs for each resolver.

A way to fix it is to use a dataloader? Not sure what it all means atm.

What are N+1 queries?

### GraphQl Server Approaches

https://99designs.ca/blog/engineering/gqlgen-a-graphql-server-generator-for-go/

There's two main things we have to consider when using a Graphql server library

1. Defining Types
2. Executing Queries

#### Defining Types

Define the types for the graphQl server

## Microservices With gRPC

[10 part tutorial series](https://ewanvalentine.io/microservices-in-golang-part-1/)

## Using gqlgen

Going through this tutorial (https://medium.com/free-code-camp/deep-dive-into-graphql-with-golang-d3e02a429ac3). Not the completely right steps because it seems sort of out-of-date.

```go
go mod init github.com/{user_name}/{proj_name}
go get github.com/99designs/gqlgen
```

Then type
```bash
gqlgen init
gqlgen generate
```

**gqlgen.yml** — Config file to control code generation.
**generated.go** — The generated code which you might not want to see.
**models_gen.go** — All the models for input and type of your provided schema.
**resolver.go** — You need to write your implementations.
**server/server.go** — entry point with an http.Handler to start the GraphQL server.
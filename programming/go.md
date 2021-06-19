# Go Programming Language

## Learning Go

https://tour.golang.org/list
https://gobyexample.com/range-over-channels

## Describes Protocol Buffer

https://developers.google.com/protocol-buffers/docs/reference/go-generated

## Examples/Route_guide

[grpc example routeguide](https://grpc.io/docs/languages/go/basics/#generating-client-and-server-code)

Just looking at the example, the cool part is it seems that writing comments in the `route_guide.proto` adds comments to the `route_guide.pb.go` and the `route_guide_grpc.go` files.

### RPC - remote procedural calls

You send a `nil` to end a RPC call. Basically it's the same even with a simple RPC `GetFeature`. Send a `return &pb.Feature(), nil`

or with streaming `return nil`

### Initialize/Declare var in conditional statement
```go
if err := x.ClientStream.CloseSend(); err != nil {
  return nil, err
}

if err := x.ClientStream.RecvMsg(m); err != nil {
  return nil, err
}
```

I think it's pretty useful to initialize/declare your variables in your conditional statement. It might seem cluttered at first but you'll get used to the language. You don't have to have additional lines to declare your variables.

## Types

### Ints

`int32` - signed int
`int` - a distinct type separate than `int32` however it is int32 size.

### Flags

Cmd line flag parsing?

### Rand

`int31n` returns a random int32 from `[0,n)` from default Source. That just means it doesn't include `n`.

## Graphql Go Examples

[Grab code repo](https://github.com/mishnit/Grab)

Has all that we're looking to implement. Uses Go microservices, GPRC, postgres, elastic search and GraphQL.

Need to go through the code to try to understand.

[Go graphql demo](https://github.com/ridhamtarpara/go-graphql-demo)

Wrote a [long article](https://www.freecodecamp.org/news/deep-dive-into-graphql-with-golang-d3e02a429ac3/) on this but I don't understand it still in the end

#to-do #grpc #go #golang #graphql

## GqlGen

### Dataloaders

https://gqlgen.com/reference/dataloaders/

What's N+1 queries?
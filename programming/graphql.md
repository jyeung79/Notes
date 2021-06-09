# Graphql
#graphql #graphql-schema #writeup

## Graphql Schema Language

graphql schema language
* define all types to query your data
* language agnostic

```graphql
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

`Character` - `graphql object type`. contains fields of data
`String!` - field is `non-nullable`
`[Episode!]!` - an array of `non-nullable` episode obj that is `non-nullable`. This means an arr of `>=0`.

### Arguments

```graphql
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```
Some fields can pass in arguments. In this case, the field `length` can pass in an arg called `unit`. It is initialized with a default value `METER`.

`LengthUnit` is the type for the field `unit`. `unit` is defaulted to `METER` when no arg is provided for `unit`. The type of `length` is `Float`.

### Queries & Mutations

```graphql
schema {
  query: Query
  mutation: Mutation
}
```

Interesting enough. You don't have to specify a name for your `queries`. I think it's the same for `mutation`. You just have to call the field name with it's args.

```graphql
type Query {
  hero(episode: Episode): Character
  droid(id: ID!): Droid
}

query {
  hero {
    name
  }
  droid(id: "2000") {
    name
  }
}
```

Mutations and queries define `entry point` of every GraphQL query. They are the same as any other GraphQL objects.

### Scalar Types

Scalars represents the leaves or the ends of a query

```graphql
query {
  hero {
    name
    appearIn
  }
}
#### Result ###
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ]
    }
  }
}
```
Basically if there are no more subfields, then it is considered `Scalar` even if it's an array.

Ex. Default Scalar Types - `Int, Float, String, Boolean, ID`

ID: Scalar type represents unique identifier and is serialized as a String. Used to refetch an object or as the key for a cache

To specify custom Scalars we define it as
```graphql
scalar Date
scalar Timestamp
scalar FoodCategory
```
We have to specify the implementation on how to serialize, deserialize and validate that type.

### Enumerations

Specify that a type muchst be a set of allowed values. Some languages (JS) has no enum support instead it's mapped to a set of integers.

```graphql
enum FriedChicken {
  CLASSIC
  SWEETANDSOUR
  HONEYGARLIC
}
```

### Lists or Non-nullable

We can specify Non-Null for arguments. This means that'll throw an validation error if we receive a null value.

```graphql
query FindNearestChicken($location: String!) {
  name
  address
}
```

Combine Lists(arrays) and Non-Null modifiers.
```graphql
myField: [String!]
### Possible Variations ###
myField: null # valid
myField: [] # valid
myField: ['a', 'b'] # valid
myField: ['a', null, 'b'] # Not valid
```
myField can be `null` but the object inside cannot. It must be a `String`.

### Interfaces

Interface means an abstract type that must include certain set of fields.

```graphql
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}

type Human implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  starships: [Starship]
  totalCredits: Int
}
 
type Droid implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  primaryFunction: String
}
```

Basically for `Human` and `Droid` type to implement `Character`, they have to contain the requried fields of the `interface Character`. `Human` and `Droid` can contain extra fields.

### Unions

`Unions` are similar to `interfaces` but they don't specify common fields
```graphql
union SearchResult = Human | Droid | Starship
```
Unions also must be concrete obj types. Not other unions or interfaces.

Query tha field that returns `SearchResult` union with an inline fragment

```graphql
{
  search(text: "an") {
    __typename
    ... on Human {
      name
      height
    }
    ... on Droid {
      name
      primaryFunction
    }
    ... on Starship {
      name
      length
    }
  }
}
### result ###
{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo",
        "height": 1.8
      },
      {
        "__typename": "Human",
        "name": "Leia Organa",
        "height": 1.5
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1",
        "length": 9.2
      }
    ]
  }
}
```
`__typename` resolves to strings which allows you to differentiate diff data types.

You can also do this where since `Human` and `Droid` share the same interface `Character`, you don't have to specify the `name` field again or any other fields specified in `Character`.

```graphql
{
  search(text: "an") {
    __typename
    ... on Character {
      name
    }
    ... on Human {
      height
    }
    ... on Droid {
      primaryFunction
    }
    ... on Starship {
      name
      length
    }
  }
}
```

### Complex Inputs

Basically you can pass in as variables more complicated structures as field arguments.

```graphql
input ReviewInput {
  stars: Int!
  commentary: String
}

### mutation
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

### variables
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}

### DATA
{
  "data": {
    "createReview": {
      "stars": 5,
      "commentary": "This is a great movie!"
    }
  }
}
```
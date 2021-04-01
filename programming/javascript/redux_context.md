# Difference between Context vs Redux

## Redux
Redux is separated into 3 parts:
1. Actions - passes in sources of information or tells the reducer what to do
2. Reducers - responsible for managing application changes when an action is made
3. Store - Store keeps track of all the global state

## Context
Context you have to do 3 things
1. Create the context using `createContext()` 
2. Define a function that will deliver the data through the Provider
3. Using `useReducer` hook to accept a reducer with the default state and returning the updated state and dispatches a function
4. Inside provider function, use `useReducer` and pass the Reducer and the initial state as arguments. The state returned and dispatch are then passed as values in the Provider

Context is ideal for small applications where state changes are minimal
Redux is perfect for larger applications where there are high-frequency state updates.

[Redux vs Context](https://www.codehousegroup.com/insight-and-inspiration/tech-stream/using-redux-and-context-api#:~:text=Context%20API%20prompts%20a%20re,a%20log%20in%20each%20component.)

#javascript #react-context #redux
### REDUX

#### State Tree
All of our states in one location instead of all over our app.

Benefits:
- Shared cache
- Predictable State Changes
- Improved developer tooling
- Pure functions 
- Server rendering

#### Store
It is a function that contains: 

- State Tree
- Get method
- Listen method
- Update method 

##### I.e.: 
  function createStore (reducer) {
    // The store should have four parts
    // 1. The state
    // 2. Get the state. (getState)
    // 3. Listen to changes on the state. (subscribe)
    // 4. Update the state (dispatch)
  
    let state
    let listeners = []
  
    const getState = () => state
  
    const subscribe = (listener) => {
      listeners.push(listener)
      return () => {
        listeners = listeners.filter((l) => l !== listener)
      }
    }
  
    const dispatch = (action) => {
      state = reducer(state, action)
      listeners.forEach((listener) => listener())
    }
  
    return {
      getState,
      subscribe,
      dispatch,
    }
  }

### Reducers
Reducers are functions that take in the current state and actions, returning a new state.

They are pure functions, they should always return the same output, no API calls, no side effects, no mutations. 

Reducers specify how the application's state changes in response to actions sent to the store. Remember that actions only describe what happened, but don't describe how the application's state changes.

Your entire application really only has one single reducer function: the function that you've passed into createStore as the first argument

### Pure functions
1. Always returns the same output if the same argument is passed in
2. Function does not produce side effects (i.e. Making an API call, Mutating data, console logs to the screen, Manipulating the DOM, Date.now() to get current date/time, async await calls/waiting for promises to resolve)

### Object.assign()
The goal in a reducer is never mutate state, always return a new copy of state. `Object.assign()` is used to return a new state object with an updated property. While effective, using `Object.assign()` can quickly make simple reducers difficult to read given its rather verbose syntax.

I.e.:
````
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
````

The `Object Spread Syntax` can replace this method and it is easier to read. It uses the spread operator `...` to copy properties from one object to another. 

````
 function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return { ...state, visibilityFilter: action.filter }
    default:
      return state
  }
}
````

### Multiple reducers
Redux allows you to have multiple reducers to handle different data (i.e. one for user info and another for handling log in)

The `combineReducers` from the redux library handles aggregation of all the reducers in the app. The aggregation is then passed to create the single redux store.

I.e.: 
````
import { createStore, combineReducers } from 'redux';  
// The User Reducer 
const userReducer = function(state = {}, action) {   
   return state 
}  
// The Login Reducer 
const loginReducer = function(state = {}, action) {   
   return state 
}  
// Combine Reducers 
const reducers = combineReducers({   
   userState: userReducer,   
   loginState: loginReducer 
})  
const store = createStore(reducers)
````


Sources:
[Redux](https://redux.js.org)

[Adhithi Ravichandran on Codeburst](https://codeburst.io/redux-reducers-are-coffee-makers-8a78dd8bb7a0)

[Tyler Mcginnis courses](https://learn.tylermcginnis.com/)

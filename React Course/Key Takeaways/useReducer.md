### Reducers in JS and React
- Reducers are a fundemental concept in Javascript and React.
- A reducer is a function that **reduces one or more complex values to a simpler one**.
	- Like the `{js}reduce()` function in JS.
![[Pasted image 20240820111753.png]]
---
### useReducer Hook
In React, useReducer is a **different way to update state, following the Redux pattern:**
- Instead of updating state directly, you dispatch actions to a **reducer function**, which determines how to compute the next state.
- useReducer looks similar to useState; it returns two values: `{jsx}[state, dispatch]`, where the first param is state (same as useState), but instead of the second param being a func that directly updates state, it is a function that **dispatches** an action, which is **then handled by the reducer function.**
- useReducer takes two args: the dispatch func, and the defualt value:
```jsx
const [state, dispatch] = useReducer(reduceFunc, initialValue);
```
#### The Redux Pattern
![[1-5.webp]]
#### Dispatch Function
- The dispatch function **should take an object** with a **"type" key** and a **"payload" key**. This is a common practice, but the keys can vary #common_practice 
- The "type" key's value is commonly an all caps string #common_practice 
- Payload can be anything.
- Example of dispatch function:
```jsx 
const [state, dispatch] = useReducer(reducerFunc, 0);

dispatch({
	type: "ADD_AMOUNT",
	payload: 10,
});

```

#### Reducer function
- The reducer function determines what should happen ot the state.
- It should be a **pure function**.
- It should be **outside the component**, so that it doesnt get re-run with the component.
	- This is OK because the function is not **linked to any value**/**pure function**.
Example:
```jsx
function myReducer(state, action) {
	if (action.type === "ADD_AMOUNT") {
		const updatedCount = state.count;
		updatedCount += action.payload;
		return {
			...state,
			count: updatedCount;
		}
	}
}
```
- The reducer function should return the new state value.

#### State in Reducer function
- **State is immutable (including useReducer state).**
	- I.e. don't modify the state in the reducer function.
	- Instead **return a new object**.
```jsx hl:5
function reducer(state, action) {
	if (action.type === "INCREMENT_AGE") {
		return {
			...state,
			age: state.age + 1,
		}
	}
}
```
#### Note:
- Usualy you won't put a function outside a component for its value will be shared by all instances of the component, unless it is a **pure function** or a value that should be shared by all components (a dict etc.)
#### Usage of useReducer:
- useReduser is for more complex state
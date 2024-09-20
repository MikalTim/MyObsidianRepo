### Different types of state
![[Untitled.png]]

---
### When not to use React Context
##### Complex Setup
- Complex apps can lead to overly "fat" context provider components, or deeply nested context components.
##### Performance
- React Context should not be used for **high frequency state changes** (for small/medium apps this might not matter)
---
### How Redux Works
- You have only one **Central Data (state) Store**.
- Components never directly manipulate the store data.
- Instead:
	- Components dispatch an **action** to a reducer func
	- The reducer function reads the action, and mutates the data in the Central Store accordingly
	- When the Central Data state is done updating, the component is notified
![[Pasted image 20240906164257.png]]
---
### Core Redux Concept

#### Reducer function
- Must be pure, no side effects (http requests etc.) 
![[Pasted image 20240909135424.png]]
---
#### Imports with node.js #js_howto 
**Note:** when using **nodejs**, you import libraries differently
```js
const redux = require('redux');
```
---
### Redux in React
#### Installing all packages:
- For redux in React, you need to install both the **reduxjs/toolkit** and **react-redux** packages.
- To install, install **react-redux**: `{sh}npm install @reduxjs/toolkit react-redux`
- Create your redux store in a **.js** file, not **jsx**, because it will not contain any JSX.
- It is a common practice to store your Redux store in a **/store** folder(like the components folder).
#### react-redux:
##### Providing the store:
- To provide the store to your components, you tipically do that in the highest up file: **index.jsx**
- To provide the Store, you should import the **<Provider/> ==component== from react-redux**, and **wrap your App component** with it.
- The **Provider component takes a ==store== prop**, which **points to the store you have created**: e.g. **./store/index**
##### Accesing the store in your components
- useSelector() hook (grabs the state form the store)
##### Dispatching actions in your components
- useDispatch() hook (causes re-render)
---
###  Destructuring Tips #js_howto  #todo 
.........

---
### Problem with original redux library
- Action {type: "increment"} - easy to make typos when dispatching
- Amount of data  managed by reducer - need to copy a lot of state on action dispatch.
--- 
### Redux toolkit
- To replace redux lib
- Extention of original redux-toolkit
#### configureStore():
- an alternative to *redux*'s createStore
- it takes an object, that has a **reducer** key/
	- reducer can be a singal reducer, or multiple reducers (if you have multiple state slices) as an object.
#### State slices:
- `{jsx}import { createSlice } from '@reduxjs/toolkit`
- When using state slice, you dont have to set if statements for your reducers
- In state slices, you are allowed to - what seems like - mutate state directly in the reducer func.
	- Redux toolkit uses an internal toolkit called **immer**, which  detects the direct state update and auto clones the state. 
	- So immutable updating is still used.
#### Using state slices
To use a state slice, you have to use the **return value of createSlice**, which is an **object**:
##### Structure of return value:
```jsx
return {
	reducer: reducerfunc,
	actions: {
		increment: (payload)=>{return {type: "auto-unique-id1", payload}},
		decrement: (payload)=>{return {type: "auto-unique-id2", payload}}
	}
}
```
##### Initiating the store with slices:
- The returned object contains a **reducer** function: `{jsx}return {reducer: reducerFunc, ....}`
- You then have to pass that reducer **to the configureStore function** to create the store.
```jsx
configureStore({
	reducer: mySlice.reducer
});
```
##### Dispatching actions to the store using **"dispatch functions"**:
- The returned object contains **actions**:`{jsx}{reducer, actions: {increment:()=>{...}, etc..}}`
	- **all ==actions== have the ==same name as the reducer's methods**==
	- **each action is a function *("dispatch function")***
- **Return of "dispatch function"**
	- `{jsx}mySlice.actions.action1(payload)` returns an object with this shape: 
		- `{jsx}return {type: "auto-gen-unique-key", payload: payload}`
		- **Note:** each unique id is linked to that actions reducer method
		- **Note:** any value passed to the action function will be added to **payload**
- **Export:**
	- You can then export these **action functions** and use them in your components
	- `{jsx} export const {increment, decrement} = mySlice.actions`
	- **you can use object destructuring to make it easier**
E.g.
```jsx hl:5,8,19
const mySlice = createSlice({
	name: "counter",
	initialState: {count: 0},
	reducers: {
		increment: (state, action)=>{
			state.count -= action.payload;
		},
		decrement: (state, action)=>{
			state.count = action.payload;
		},
	}
});

const store = configureStore({
	reducer: mySlice.reducer
});

export default store;
export const {increment, decrement} = mySlice.actions
```
	-Note: the return value of actions have the same name as the reducer methods
#### Working with multiple slices
- configureStore can take multiple reducers as an object (each reducer is given a key)
- So you can create an extra state slice and put it into configureStore.
```jsx
const store = configureStore({
	reducer {count: countSlice.reducer, auth: authSlice.reducer}
});
```
- **Note:** now when accesing state in your component, you have to use the key you used for the reducer: **state.reducerKey**
```jsx 
const state1 = useSelector(state => state.key1.amount);
const state2 = useSelector(state => state.key2.value);
```
#### Splitting store code
- You can put every slice in a new folder, in the **/store** folder.
- And import it to your **/store/index.js** file, where you create the store.
- You can import the **state slice's actions** into your component from every seperate **state slice js file**.
---
### [[Advanced Redux]]
---

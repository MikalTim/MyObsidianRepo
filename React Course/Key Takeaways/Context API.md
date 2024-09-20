### Context API
- Context lets you manage state and shared data across components without using prop drilling
![[Pasted image 20240812101504.png]]
---
#### Create Context
- First you need to **create the context** using ==**createContext**==
- Call `createContext` outside of any components to create a context.
```jsx
import { createContext } from "react";

const MyContext = createContext("light");
```
- You can create this in a sepparate file (like a JSX component). 
- You can also create a folder called **store** for all contexts(like the *components* folder). #best_practice 
- Start the 
##### Parameters:
- `{jsx}defualtValue`: 
	- this value is only helpfull for autocompletion when calling the created context.
	- meant as a "last resort" fallback.
	- it is static and never changes
---
#### Provide Context
- **Wrap the components** you want to give context to in the **context provider**.
```jsx
import { MyContext } from "./store/MyContext.jsx"

function App() {
	return (
		<MyContext.Provider value={myvalue}>
			<MyComp/>
		<MyContext.Provider>
	);
}
```
- **Note:** `{jsx}createContext` creates a **JS object** that contains a **JSX component** - which is the ==**context provider== -** `{jsx} MyContext.Provider`
	- This is used like this: `{jsx}<MyContext.Provider></MyContext.Provider>`
##### Provider Props:
- **value**: the value you want to pass to all the components inside the provider.
	- this can be any value and can contain state
---
#### Consume Context
- Inside the component the context is provided to, use the **useContext** hook to "consume" the context.
```jsx
import { MyContext } from "./store/MyContext.jsx"

function Button() {
	const ctxValue = useContext(MyContext)
}
```
- useContext takes the context as a variable, i.e. you have to import the context.
---
#### What happens when you change Context values
- Context causes all the components it contains to re-render when its value changes
---
### Outsourcing Context & State to separate "Provider Component"
- You can outsource the context and state values to a new component in a sepperate file.
- E.g.
```jsx
const ContextProvider = ({ children }) => {
	const [state, setState] = useState({items: []});
	
	const ctxValue = {
		item: state.items
	};
	
	return (
		<MyContext.Provider value={ctxValue}>
			{children}
		</MyContext.Provider>
	);
}
```
---

---
#### Reference:
1. [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)

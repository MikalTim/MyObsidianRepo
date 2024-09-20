### React Refs 
> "When you want to store information in your component, but dont want to trigger a re-render, you use a ref"
- Refs are an React escape hatch.

#### Referencing values with Refs
- You can **add a ref to a component with** the **useRef Hook**.
- You can **assign a initial value to** the **ref** by **passing it into useRef()**; e.g. `{jsx}const ref = useRef(0)`
- **useRef() ==returns== a object with this structure:**
```jsx
{
	current: 0 // value passed to useRef
}
```
- A **ref** ==persists== **through re-renders**.
- You can **read** / **change**, the value of that ref through **ref.current**
- You can **point** the **ref to any value**, however it is **mostly used to access a DOM element**.
- This is **done by passing** your **ref to** a **HTML component's** ==**ref prop**== in JSX- `{jsx}<input ref={myRef}/>
	- **This updates the ref to point to that HTML element.**
	- You can access properties of that component like: value, text, etc.
#### Refs vs State

| state                                                                                  | refs                                                    |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| triggers a rerender when you change it                                                 | doesn't trigger a re-render when you change it          |
| **Immutable** - must use state update func to change                                   | **Mutable** - can be modified outside rendering process |
| **Should not be used for "behind the scenes" values that have no direct impact on UI** | Should be used for "behind the scenes" values           |
#### Connecting to HTML DOM with refs 
- You have to **declare a ref with** the **useRef hook**.
- Every HTML component in JSX has a **ref prop**
- You have to **set the ref hook as a HTML elements ref prop**.
```jsx hl:2,4,6
const App = () => {
	const name = useRef()
	
	name.current.value = "Hello World";
	return (
		<input ref={name} type="text"/>
	);
};
```
- You can then **read/write** any **property contained in the HTML component**, e.g. value, onClick, etc.
- **Note:** to access the HTML element with useRef(), you have to use`{jsx}ref.current`

### Summary of Refs:
- **useRef() creates a ==special object== `{jsx} { current: someValue }`, that persists across re-renders.
---
### Forwarding Refs
- In React, refs cannot be forwarded to custom components simply as props.
- You have to use the React function - **forwardRef()**
#### forwardRef()
- **forwardRef()** wraps around your component (i.e. it **takes your component/function as a variable**) and **returns a component with the ref forwarded**.

#### forwardRef parameters:
- **render**: the render function for your component new component that forwardRef will create.
	- just your old component function passed as a variable.
```jsx
const MyInput = forwardRef(function MyInput(props, ref) {});
```
- **Your component("render" parameter) should take 2 variables:** **function MyComponent(==props==, ==ref==)**
	1. The components props
	2. The ref that should be passed to that component

- It aplies the ref to your component.
```jsx
const MyComponent = forwardRef(function MyComponent(props, ref) {
	return <input ref={ref}/>
});
```
- Note, your component should be modified to have two inputs, because forwardRef will pass 2 inputs to it.
---
#### Note:  #js_howto
- When you have `{jsx}A ? A : null`, where the statement, **if A is true, returns A**, JS has a shortcut:
	- `{jsx}somevalue ?? null`
	- This reads: if A is true, return A, else return null.

#### Note: #good_practice  
- When creating a component in a seperate JS/JSX file, any Global vars in that file will be shared by all of the component instances
- This is not usefull when that var is a timer, for once one instance starts the timer, the timer is started on all.
---
### React portals
- lets you **render** some **children** into a **different part of** the **DOM**
- part if the ==**react-dom**== **API**

#### Creating a Portal 
- Import createPortal from "react-dom".
- createPortal() takes 2 parameters:
	1. The children you want to teleport. Can be any JSX.
	2. A DOM node from index.html to teleport to. (e.g. the body, a div with a id etc.) 
```jsx
function MyApp() {
	return (
		<div>
			createPortal(
				<MyComp></MyComp>,
				document.getElementById("MyReference")
			);
		</div>
	);
}
```
#### Note
- Events from a portal **propagate according to the React trea rather than the DOM tree**
---
#### Note: #definition 
- A **Modal** is a web page element that displays in front of and deactivates all other page content (...you might want to teleport this to the main DOM body or somewhere there)
---
#### Reference:
1. [React Refs](https://react.dev/learn/referencing-values-with-refs)
2. [createPortal](https://react.dev/reference/react-dom/createPortal)

### useEffect:
- Used to handle **side effects** (only in **certain scenarios**).
- Should **not** be **used for opperations that run synchronysly** (i.e. that run during the render cycle) but for **opperations that are asynchronous.**
- Should **only be used** if your **side effect** can **cause an infinite loop** or if you have code that can **only be executed after the component function (async)** has executed at least once.
---
##### Side effects: #definition 
- Any opperations that occur in a component that doesnt directly impact the current component render cycle.
- Occurs after rendering. 
	- Asynchronous
---
#### Side effects outside the Component
- Some side effects that you want to run only once can be placed outside your component so it is only run once.
---
#### Notes:
- Only use useEffect when you need to.
- Never use useEffect for synchronous operations.
---
#### Component Life Cycle
1. Mounting - component starts existing in the UI.
2. Updating - component is re-rendered / updated (state changed)
3. Unmounting - component removed from the UI.

#### Effect dependancies
- Prop or state values the effect depends on / i.e. prop and state values used in the effect or that should trigger the effect to re-render
- All the state and prop values used in the effect should be included in the dependancies to insure it uses the latest version of them.
	- otherwise the state/prop vars your using might be from a previous render, because the effect function is not run again, and thus still use the old vars

#### useEffect params:
- **useEffect** takes in:
	1. A function (to be run when the Effect is triggered)
	2. An array with all the dependancies (will trigger the effect when updated)
- Inside the function you have the code to run on the effect trigger.
- And the function should return the **cleanup function**.
- An empty array defines no dependancies 
```jsx
useEffect(() => {
	const timer = setTimeout(()=>{"do this"});
	return () => {
		clearTimeout(timer);
	};
},[dependancy]
);
```
##### Note: 
- If the dependancy array is omited, the effect will be triggered every time the component is re-rendered.
#### Cleanup Function
- Function returned by the useEffect **param function**.
- Runs every time:
	1. useEffect runs twice
	2. The component dismounts

---
### Danger of using Functions as dependancies:
- Functions are objects.
- They are re-run / re-created every time the component is executed again.
- Even though the function(object) structure doesnt change, it is not the same; points to different memory registry.
- This can thus cause an infinite loop:
E.g.
```jsx
function App() {
	const [state, setState] = useState(0);
	
	const handleUpdate = () => {
		setState((prev) => prev+1)
	};

	useEffect(() => {
		handleUpdate()
	}, [handleUpdate]
	);
}
```
	- handle update will trigger a re-render when useEffect is run, and because handleUpdate is changed, useEffect will re-trigger.
- This happens when the function(object) triggers a state update (i.e. re-render) that  re-renders the component the function is in.
	- However, e.g. in the case the function also removes the component( dismounts it ), it is safe.

### useCallback:
- wrap around function you want to pass to useEffect
- useCallback returns the function you wrapped, but now it will not be recreated each time the component is re-run

Paramameters:
- the function you don't want to re-create on component re-run.
- an array of dependancies.

useCallback dependacies:
- In dependancies aray, you should add all prop and state values that are used within the function
- Note: state updating functions are not neccesary
- Like in useEffect, your useCallback function wil  only be run if the dependancies are updated.
- Good practice to always use useCallback for state updating functions, to ensure extra safety.
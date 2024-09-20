### Dynamic content in React
- In React, ==dynamic content is placed in **{}**==
E.g.
```jsx
<div>{dynamicValue}</div>
```
#### Loading images in React
- Loading images with `{html}<img src="dir">` is not the best way in React
- Rather, use ==import==
```jsx
import image from 'img.jpg';
```

---
### Reacting to events

#### Example in Vanilla JS
- in vanilla JS, to add an action on a event (e.g. button click) you would use:
	o	``{js}document.gquerySelector(‘button’).addEventListener(‘click’, myFunc);
-	this ==relies on **JS WebApi** to call the function from the queue stack, and make it non-blocking and async==

#### Using React
-	in jsx you can use the react onClick prop (e.g. in a button)
	o	`{jsx}<button onClick={mFunc}>ClickMe</button>`

### Passing custom args to Event funcs
- the usual syntax: `{jsx}onClick={myFunc}`, ==doesn’t allow you to pass custom params to the event function==.
- this is because when you pass the func as `{jsx} myFunc(...args)` in **onClick={}**, ==it runs the function imedietly==, 
- to pass args to your func, you have to embed it in another func, `{jsx} () => {myFunc(...args)}`, becuase now the function is being "***called"***
E.g.
```jsx hl:5
const myFunc = (...args) => {
	console.log(args)
}

<button onClick={() => {myFunc(1,2,3)}}></button>
```

#### Event Arg
- Like in normal HTML, onClick passes the **event** object to this function, which you can use with **event.target**.
```jsx hl
onClick={(e) => console.log(e.target)}
```

---
### [[React Props]] #on_event

### [[React Hooks]] 

---
### Conditional Rendering #conditional_operators
- ==In JSX code, you can add conditional operators== (==**not statements**==) to render content only on a condition, if you put it in {}
```jsx hl:3
function App() {
	something = true;
	return {!something ? <p>Something is Empty</p> : null};
}
```
- Examples of these operators:
	- **?** - if/else
	- **&&** - if

--- 
### Dynamic lists in React #map_func
- to display list data dynamically in JSX, you can use **.map()** to iterate over the list, and display something for each item
- **Note: you cannot use for of**
```jsx
myList.map((x) => <div>{x}</div>)
```
- When using this approach, if an item is removed from te list, it wont break, but simply not display it

#### The Key prop
- The ==**key prop**== is a ==**built** in **React** **prop**== and is ==**necessary** when== you **==render a list dynamically**==
- **key helps React identify each item** (which items have been changed, added, removed).
	- **It gives each element a stable identity**
- It should be a unique ID contained within the list
---
### [[CSS in React#CSS & Dynamic styling| CSS & Dynamic Styling]]
---
#### Best Practice: Updating State based on old State #best_practice
- In React, **when updating** the **value** of a **state based on the previous state** (e.g. **!someOldState**):
	- It is **highly recomended** to **pass** a **function** **to** the **state updating function** (set*XXX*) and **(not e.g. the ! opperator on the old state)**
	
| Not Recomended                   | Recomended                                                            |
| -------------------------------- | --------------------------------------------------------------------- |
| `{jsx} setIsEditing(!isEditing)` | `{jsx} setIsEditing((editing) => !editing)`                           |
|                                  | Here, The **state updating function passes a value to the function**  |
|                                  | The **function** is **not directly executed like**  `{jsx}!isEditing` |

##### This is because:
- **React schedules state updates:**

| Somwhere in your code       |         | Reacts State updating schedule |
| --------------------------- | ------- | ------------------------------ |
| `{jsx} setIsEditing(true)`  | **-->** | 1. Update state to True        |
| `{jsx} setIsEditing(false)` | **-->** | 2. Update state to False       |
| `{jsx} setIsEditing(true)`  | **-->** | 3. Update state to True        |
| `{jsx} setIsEditing(false)` | **-->** | 4. Update state to False       |
- **State updates are not performed instantly**
- It takes a few milliseconds.
- So when calling:
```jsx
setIsEditing(!isEditing);
setIsEditing(!isEditing);
```
-  **The state might not be updated yet when the second update is called**

##### Why you should use a function instead
- Using a function:
```jsx
setIsEditing((editing => !editing);
```
- Because the function will not be executed directly, but when React schedules the update to be.
- Note: passing !value is the same as directly executing the function in the state update func: `{jsx}setIsEditing(((editing) => !editing)())`
---
### Input and Two way binding
- **Two way binding:** a pattern where changes in the UI are reflected in the state, and changes in the state are reflected in the UI. 
- **In HTML, form elements such as** `{html}<input>, <textarea>, <select>`typically **maintain their own state and update based on user input.** 
- **You can combine this state into your React state by using two way binding.**
- **When combining the two**, you **make** the **React state** the **'single source of truth'**.
- An input form element whose value is controlled by React in this way us called a controled component
##### Controled components #definition
- A form input component whose value is controled by React with state.
##### E.g. of two way binding 
- The value prop of an input in React, if set, will overwrite any input from the user.
	- `{jsx}<input value="Hello" onChange={myFunc}/>`
	- The value "Hello" will overwrite any attemted intput
- Here, two way binding can be used to update the value of the input.
```jsx
<input value={state} onChange={myFunc}/>

const myFunc = (event) => {
	setState(event.value)
}
```
---
### Update Object State Immutably #best_practice 
##### When you use an array or object as a state:
- You **have to update** the **state immutably** (i.e. **make** a **copy of** the object/array)
	- This is because **objects and arrays are ==reference variables==**.
	- When you **update them**, you **update** the **memory referenced by them** and...
	- So you **wont trigger** a rerender **(because the state memory/var has not been edited)**
- (so) **In** the **setState**, you have to **pass** a **copy of the object/array** (**pass a new var/ new memory reference**, so **rerender** is **triggered**)
- **Else**, the **state will only update on** the **next render, triggered by another state**.

#### Note on spread opperator(...)
- The spread opperator/syntax is "shallow" / only copies one level deep.
- Creates a new address location only for top-level elements.
	- I.e.`{jsx} const A = [[1,2], [1,2], [1,2]]` spreaded will look like this(**remember arrays are objects**) `{jsx} [...A] = [Array1, Array2, Array2]`
- This makes it fast, but you'll sometimes have to use it more than once
- **E.g**
```jsx
const A = [[1,2], [11,3], [4,6]];
const B = [...A.map((innerArray) => [...innerArray])
```
	- map() -> for every object in A spreaded, spread the item into an array

- You can use [Immer](https://github.com/immerjs/use-immer) for easier, more consise syntax in copying nested arrays/objects.
---

### Lifting up state #core_concept
- **Sometimes**, you **want** the **state of two components** to **change together**.
- To do it, **remove state from both** of them, and **move it** to the **closest comon parent**, and **pass it down via props**.
- **Note**: you can pass down the state and the state changing func
E.g.
```jsx
const A = ()=> {
	const [myState, setMyState] = useState("");
	return (
		<MyComponent1 state={myState}/>
		<MyComponent2 stateSet={setMyState}/>
	);
}
```
--- 
### Principles for structuring state
##### See [Choosing state structure](https://react.dev/learn/choosing-the-state-structure)
#### 1. Group related state:
- If you always update two or more state vars at the same time, consider merging them into a single state var.
- **E.g. Should you:**
	`{jsx}const [x, setX] = useState(0); const [y, setY] = useState(0);` **OR/**
	`{jsx}const [pos, setPos] = useState({x: 0, y:0})`
	- **If the states update together, group them into a single state.**
#### 2. Aviod contradictions in state
- If state is set up in a way that several pieces of state may contradict, you leave room for mistakes.
- **E.g.** If you have created two state vars that break if you don't call them together, this causes unnecessary complexity, and room for mistake
#### 3. Avoid redundant state
- If you can calculate some information from the components props or its existing state vars during rendering, you should not create a new state for that state.
#### 4. Avoid duplication in state
- When same or similar data is duplicated between multiple state vars, or within multiple objects, it is difficult to keep them in sync.
#### 5. Avoid deeply nested state
- Deeply nested state vars are not very convinent to update.
- **E.g.** 
```jsx
const Data = {
	0: {
		id: 0,
		user: {
			name: "Mike",
			age: 25
		}
	},
	1: {
		id: 1,
		user: {
			name: "Ike",
			age: 45
		}
	}
}
```
- **Restructure** the **data** of the state **to be more flat**, I.e **normilize the data**
---
##### Remember: 
- Add **key** element if you want to map out an array in React. 
- This is required by React.
##### Remember
- A arrow fuction with {} needs the return statement.

---
### Some best Pracites #best_practice 
#### Why immutability matters
- Best to **make all vars used as state**,(**or in conjunction with states**) **imutable**, **otherwise** there might be **hard to find bugs** in your system.

### When not to lift up state
- You should not lift up state when that state will then cause other parts of your app to reload that you dont want to reload.
---
#### Reference:
1. [Update Objects in State](https://react.dev/learn/updating-objects-in-state)
2. [Spread Opperator](https://dev.to/mlgvla/javascript-using-the-spread-operator-with-nested-objects-2e7)
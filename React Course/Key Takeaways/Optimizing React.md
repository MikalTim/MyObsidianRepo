using Dev tools to see what components are re-rendered when what state is updated and how long each component took

### memo()
- If you have a high up state, and it changes React will re-render all the child components
- If those components do not change during the re-render, they should not be re-rendered, because it costs performance.
- This is where you can use **memo()**
- **memo()** lets you skip re-rendering a component when it's props are the same.
- **Note**: memo() does a check on the cached props and the current props, which uses computation.
![[Pasted image 20240902155756.png]]
#### When to use:
![[Pasted image 20240903145613.jpg|500]]

---
##### Note:
- **State update functions provided by React hooks are guaranteed to maintain their identity across re-renders, unlike normal functions defined within a component**
---
##### Note:
- Use memo() might update on every re-render because one of its props is a function, which changes on every re-render:
	- In this case you should use useCallback() to stop the function from doing that.
---
#### useMemo() 
For normal functions you dont necessarily want to recalculate every component re-render
Should only be used for functions that do complex calculations

useMemo structure
- Takes in  a anonomous function that returns the function you dont want to re-execute every re-render
- It also takes a dependancies array; the dependancies your function needs

useMemo functionality:
- useMemo will then only execute your function when the functions dependancy changes (dendancies in the dependancy array)
- Empty dependancy array will make the function never re-execute on component mount

Note: dont overuse useMemo, like memo. Should only be used if your function is compute intensive

---
Virtual Dom.
![[Pasted image 20240903113938.png]]
- Lives **only in memory**
- Virtual DOM executes the React components (that have been re-rendered / state triggered) every time and creates a virtual dom, which then gets compared to the current DOM and check for HTML elements that have changed, and only updates them.
	- Creating this virtual DOM is still faster because it is only created in memory.
![[Pasted image 20240903120028.png]]

---
### Keys and Managing State
- **State** is **scoped to** *==a==* **component**
	- *it is re-created when ever you use a component*
- React **identifies** (multiple of the same) **components** by **position** and **key**. 
```jsx
<Menu/>
<Menu/>
<Menu/>
```
	- this can be refered to as a component array
 - Actualy, **when key is left empty**, the **component index(position) is used as the key**.
 ```jsx
 <Menu/> === <Menu key={0}
 <Menu/> === <Menu key={1}
```
- React uses the **key to know which component** ==a state== **should be mapped to**.
- *When a component in a array changes position, and position(index) is used as the key, the state will be asigned to the wrong component.*
- **Note:** do not set key to Math.random(), because then the component will never have a concrete key across re-renders
```jsx
!!! `Do no do this` !!!
{people.map(person => 
	<Person key={Math.random()}>
)}
```
- Rather than generating keys on the fly, you should include them in your data:
```jsx hl:2
export const people = [{
  id: Math.random(), // Used in JSX as a key
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}
```
#### Performance reasons for using Keys
- During the Virtual DOM compare, a component will be interpreted by React as unchanged if its key is the same as the last render.
- So if your using index as your key, and you update the component list, the entire list gets re-rendered in the real DOM because all of them will have new keys.
	- This is not optimal, even if state tracking is not a problem
- But if each item has a **persistant** unique id, only the updated item will be re-rendered.

#### Resetting components with keys
- When a component key changes, React will re-create the component (unmount then re-mount / i.e not re-render)
---
### Arrays
- You should treat arrays in React state as read-only.
![[Pasted image 20240904122338.png]]
---
### State Scheduling
- When a state is updated, a re-render will be **scheduled**
- *Which is why you pass a function to a state updating function if the state depends on a previous state, so when you have multiple consecutive state updates, the old value is not used* #best_practice 

#### State batching
- When you have multiple state updates that are triggered *"simultaniously"*(consecutively), React won't re-render the component for all the state updates.
- Because React performs **state batching**.
---
### MillionJS
- Optimizes the React virtual DOM
- Look into it #todo 
#### Resources:
- https://react.dev/learn/rendering-lists
	- https://react.dev/learn/rendering-lists#rules-of-keys
- https://react.dev/learn/updating-arrays-in-state
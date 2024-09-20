### Description
- Hooks are a way to "hook into" React Features
- All react functions that start with **useXXX** , are react hooks.

### How React components are executed
- ==React components are exectued only once, unless the state changes==

### Rules of hooks
- Hooks can ==only be called in react components, or inside other react hooks==.
- Hooks ==cannot be called in a function or statement that is nested in a react component.==
	- I.e. a hook must be called ar the top level of a component
----
### Using the useState hook
- ==useState()== is a Hook that ==lets you add React state to a component==.
	o	==state: where you store property values that belong to the component==.
- **What does useState() do:**
	o	It ==declares a “state variable”==.
	o	this is ==a way to “preserve” some values between component calls==.
- **What args do we pass to useState():**
	o	**one arg: initial state**
- ==useState() returns a array which always contains 2 elements:==
	o	==current state value.==
	o	==state updating function()==, **which also** ==reloads the component the hook is in==.

### useState under the hood:
- when the state updating func is called (set…()) - **useState()** is used like this: **react schedules** an **update** for the state, which is only ==**done once** the **Component** is **reloaded.**==
---
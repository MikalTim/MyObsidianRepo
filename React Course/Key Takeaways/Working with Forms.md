### Handling Form Submission
- In HTML, a form element, by default, handles submitions by sending a HTTP request to the host server and, reloading the page.
![[Pasted image 20240905154942.png|650]]
- However, in React, this is not optimal (unless you have a Next.js server backend).
- Rather, the form prop **"onSubmit"** should be set to a custom function.
	- The function is passed a **"event"** prop
```jsx
<form onSubmit={(event)=>{console.log("Sumbited")}}/>
```
In the function you should set `{jsx}event.prevenDefualt()` to prevent the form its defualt actions.
```jsx
const handleSubmit = (event)=> {
 event.preventDefault();
}
<form onSubmit={handleSubmit}>
	<input/>
	<button/>
</form>
```
![[Pasted image 20240905154835.png]]

### You can get the form input with:
- State and two way binding: [[Dynamic Content and State#Input and Two way binding | Two Way Binding]]  
- A ref with useRef.
- The native browser feature: FormData.

#### FormData
- The browser helps you by allowing you to create a special **object** based on a special **constructor function**: `{jsx}new FormData`.
- You can pass the form to FormData(): `{jsx}new FormData(event.target)`, **in the form onSubmit func**.
- You can then **use methods provided by the FormData** object like `{jsx}.get()` to get the form data.
	- **Note:** `{jsx}.get()` takes the **input name** as a parameter to know which input to return the data from.
```jsx
const formData = new FormData(event.target);
formData.get('email');
formData.get('password);
```
- However this can be tedious in complex forms, so you can use the JS built in object`{jsx} Object` and  FormData's `{jsx}.entries()` function: 
	- `{jsx}const data = Object.fromEntries(formData.entries())`
	- `{jsx}FormData.entries()` returns an array off all input fields and their values.
	- Performing `{jsx}Object.fromEntries()` on the array returns an object with key, value pairs for all the input fields.
	- However, when you have e.g. a group of radio buttons with the same name, that data will be lost during `{jsx}Object.fromEntries()`:
	  - You can use another function of FormData, `{jsx}.getAll()` to get all inputs off a **name**.
	  - **getAll** **returns an array** of values whose key matches the specified name.
		  - `{jsx}const radioBtns = FormData.getAll('this-name')`

#### Resetting Forms
- You can just use a button in the form set to: **type="reset"** - `{jsx}<button type="reset"></button>`
- Or, if you where using State to store input, you can just reset the state value.
- And if you are using refs, you can reset the input value: `{jsx}current.value=''`
- You can also reset the form through the form onSubmit func: `{jsx}event.target.reset()`
	- The 2 above are not recomended, because they does not depend on React to update the DOM / Sepperated from the React cycle (because they are not connected to state)

#### Validating Inputs
![[Pasted image 20240906102837.png]]
##### You can validate your input by using state
- This is custom form validation
##### By using Built in browser functionality
- https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation
- These are built in HTML component features for form validation.
- A combination of these and custom validation can be used.

##### By using Third Party libraries
- You can use some 3rd party React form libraries like:
	- [React Hook Form]() : Easy to use, lightweight, min boilerplate(built ontop of React hooks), performance.

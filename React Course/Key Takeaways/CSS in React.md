### Style in React
- React components have a **className** and **style** **prop**, which can be **used to apply CSS** to your components.
- **className**: **JSX version of HTML class.** `{jsx}<input className="btn"/>`
	- **Accepts** a **string**, **or** a **dynamic value**, e.g. `{jsx}<input className={myClass}/>`
- style: for inline style in JSX. `{jsx}<input style={}/>`
	- **accepts a dynamic value**(style={}**) which should be a **object/dictionary** (**style={ {} }**). E.g. 
		`{jsx}<input style={ {textSize: "10px"} }/>`
- Remember that **React relies on camelCase**. Thus **all style attributes are written as cameCase**, with all dashes removed (**text-align --> textAlign**)
---
### Vanilla CSS in React

| Pross                          | Cons                                 |
| ------------------------------ | ------------------------------------ |
| CSS code is decoupled from JSX | CSS code is not scoped to components |
| Universally  used              |                                      |
|                                |                                      |
#### CSS code is not scoped
- If you import CSS into a specific component, it will affect all your JSX code from other components
- This means your classes can clash

#### Inline CSS
- Every component in JSX has a **style prop**. It **takes in** a **dynamic value** (so **style={}**) which is a dictionary (so **style={ {} }**)

| Pros                           | Cons                                                   |
| ------------------------------ | ------------------------------------------------------ |
| easy to add to code            | you need to style and target every element individualy |
| is scoped                      | no sepparation of code                                 |
| dynamic styling is made simple |                                                        |
#### Note on CSS in JSX (inline)
- In JSX ,all style attribute names follow camelCase and dashes removed.
	- I.e. style atributes like **text-align** would be **textAlign**
---
### Dynamic CSS
##### Classes:
- You can dynamically set a class based on a state.
```jsx
<input className={isTrue ? active : inactive}/>
```
- **Note**: when setting dynamic content, **do not use "&&"**, for that will set the class, if the var is false, to false and not undefined - and this gives an error.
- When setting style dynamically, **always use undefined instead off null**, 

##### Multiple classes
- If your element has multiple classes and one of them is dynamic, you can **use the JS special =='${}'== string interpolation** to **make it into a single string**.
```jsx
className={`label ${red ? "red":"green"}`}
```
##### Inline CSS
- You can also make inline CSS dynamic. E.g.
```jsx
const [myColor, setMyColor] = useState("")
const myBackgroundColor = 
style={ {color: myColor, backgroundColor: myBackgroundColor} }
```

---
#### Quick Note:
- **&& used as an if statement automatically returns false if the statement is not true**

---
### Scope with React's ==CSS Modules==
##### CSS modules
- **CSS modules allow your CSS to be scoped to your React components.** 
- **This allows you to create a sepparate CSS files for every component (which will not effect the other components.)**
- ==Modules are just **.css** files with an extra **.module** added, e.g-> **MyFile.module.css**==
##### How they work
- This **tells** the **React build process** to **procces them differently**, **and to give scope to the CSS**
- The build process creates a **JS object** that **stores all the style classes**, so it **should be used and imported like a JS object. E.g.**
```jsx hl=1,4
import classes from './MyComponent.module.css';

const App = ()=> {
	return <h1 className={classes.myclass}>Hello World</h1>
}
```
- **Note:** **use capital letters**(like for components) **for module files** coresponding to the component they style.
- **Note:** you can use Sass files as a module.
- **Note**: only **css classes can be exported**.
---
### Tailwind CSS #refactor-to-newfile

#### Instalation: #instalation
##### Vite:
1. **Install dependancies:**
- In the terminal, **Install** tailwind css: `{sh}npm install -D tailwindcss postcss autoprefixer
- **Generate** **'tailwind.config.js'** and **'postcss.config.js'** files:`{sh}npx tailwindcss init -p`

2. **In *tailwind.config.js*:**
- Add the **paths to all of your template files** in your **tailwind.config.js** file.
```js title:tailwind.config.js
content: 
	[ 
		"./index.html", 
		"./src/**/*.{js,ts,jsx,tsx}", 	
	],
```
3. **In your CSS**
- Add **@tailwind** directives for each of ==**Tailwinds layers**== to your **'./src/index.css'** file.
```css title:index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
4. **Start the build process**
- In the terminal: `{sh}npm run dev`

5. **Then start using**
#### Dynamic content with tailwind
- Because the tailwind styling is aplied with a string, the string can be stored in a var, and you can concat, edit the string based on some value/state.
- You can also use Javascript template literals.
**E.g.**
```jsx
let mystyle = `m-4 p-4 ${invalid ? "color-red" : "color-blue"}`
className={mystyle}
```
---




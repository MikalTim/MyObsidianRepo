### What is props
- a ==dictionary== that stores the  kwargs in your component - 
	- `<Component item1=0 item2=1 \>
- Acces it in your component(function) `{jsx} (props) => {props.value}`. 
E.g.
```jsx hl:1
const Item = (props) => {
	console.log(props.value1);
	console.log(props.value2);
}
```

### Using destructuring to access props
- You can object "{ }" destructuring to acces the props in a more concise manner:
- In your component(func) `{jsx} ({value1, value2}) => {value1 - value2}`
E.g.
```jsx hl:1
const Item = ( {value1, value2} ) => {
	console.log(value1);
	console.log(value2);
}
```

### Defualt values for props
- When destructuring, you can give props defualt values
```jsx hl:1
const Item = ( {value1, value2=10} ) => {
	console.log(value1);
	console.log(value2);
}
```

### Special Children prop
- children prop: refers to the ==content between your component opening and closing tags==.
- Set by React
- Can be **text**, or **complex JSX structure** (with div, h1 etc)
E.g.
```jsx hl:1,3,10
const Button = ( {value1, children} ) => {
	console.log(value1)
	return (
		<button>{children}</button>
	);
}

const App = () => {
	return (
		<Button>This Goes To children</Button>
	);
}

```

### Defualt Prop value
- You can give props a defualt value when destructuring
```jsx
({x=0, y=10}) => {console.log(x,y)}
```

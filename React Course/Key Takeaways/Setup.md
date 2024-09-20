## Creating a local React App
## With Node.js
- You need to use node.js to create React apps.
- It handles the environment and packages necessary for React
- There are 2 ways in Node.js which can be used to create a React app :

#### **Using create-react-app:**
-	This method allows creating a react app without polluting global node packages
Run this in a terminal (if windows: make sure you are using admin cmd):
-	npx create-react-app app-name
-	cd app-name
-	npm start app-name

#### **Using Vite:**
-	**Vite is a local development server for Vue and React projects.** It supports Typescript and JSX.
-	Vite  is a build tool that aims to provide a faster, leaner development experience for modern web projects. It consists of two major parts:
		o A dev server.
		o A build command that bundles your code with Rollup, pre-configured to output highly optimized static assets for production.
-	It allows HMR (hot module replacement)

==Run this in a terminal:==
-	run this to create the project
	o	npm create vite@latest project-name
		- It will ask you with kind of project you want to create. Choose React.
-	run this once initialy to download the required packages:
		o npm install #vite_command
-	run this to launch the Vite server:
		o npm run dev #vite_commanf

### Note on .JSX extention
- .jsx is not supported by the browser
- it's used in this project because the project supports this special extention (with Vite)vite
- Some React projets use the .js file extentions
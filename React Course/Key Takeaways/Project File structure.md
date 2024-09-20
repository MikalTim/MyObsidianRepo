### React file structure
- components should all be stored in separate files
- all components (except the root App) should be stored in a /components folder
- component files should preferably contain only 1 component
- the file should be named after the component
- component styles can also be separated and stored next to the component folder, or in a named folder to make it easier to find related styles.
#### Imports 
- to import from the current folder use a single dot before /
- E.g –  ./file-to-go-to
- to import from a folder above the current folder use 2 dots before /
- E.g –   ../file-to-go-to

### Public vs Assets folder

#### public
- made publically available by the underlying project dev server and build process
	- just like index.html, those files can directly be accesed from inside the browser and can be requested by other files.
#### src/assets
- not made public 
- can only accessed during the buid process 
	- i.e used in code files
#### Which to use
- public: any files that should not be handled by the build process and that should be generally available.
- images etc. used inside component (thus the build process) should be stored in the src folder (e.g. in **src/assets/**)
---
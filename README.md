# React crash course

## Requirements
- Node.js
- A text editor: Visual Studio Code

## Setup
- Check if Node.js is installed
```console
node -v | npm -v
```
- Then, create a React project, using
```console
npx create-react-app <app_name>
```
- Start the development server
```console
npx start
```
- For emmet to work, change that language from JavaScript to React JavaScript (below terminal)
- If using the Visual Studio Code, aslo install ES7+ React by dsznajder, will provide more emmets
- For default arrow function that exports a component, use
```console
rafce
```
- Use React icons running: ```npm i react-icons``` in terminal.
- To get the build, close the development server and run:
```console
npm run build
```
- To serve this build locally, run:
```console
sudo npm install -g serve
serve -s build -p 8000
```
Notes:
    - Use ```sudo``` if using macOS
    - ```-p 8000``` for using the 8000 port; it should copy the address to clipboard by default
- Mocking a JSON server for development
  - Install it locally
  ```npm i json-server```
  - Add a new script to ```package.json/scripts```:
  ```"server": "json-server --watch db.json --port 5000"```
  Note: Or any available port.
  - Run ```npm run server```
  Notes:
    - If it is the first time running it, add ```db.json```
    - Make sure you are in the app's folder when running the commands in the terminal.
  - Then, start the development server.
- Install React routing library: ```npm i react-router-dom```

***

## React Syntax
- Classic component
```jsx
const <component_name> = ({ props }) => {
  return (
    <header>
        <h1>Task Tracker {props}</h1>
    </header>
  )
}
```
Notes: props are parameters, they are optional. They can also have types;
- Props types
Import prop types using
```jsx
import PropTypes from 'prop-types'
```
or using ```impt ``` to generate the same thing.
Then assign a type:
```jsx
Header.propTypes = {
    title: PropTypes.string.isRequired
}
```
Note: Add ```isRequired``` only if (obviously) the type is required :)
- Default prop values
```jsx
Header.defaultProps = {
    title: "Task Tracker",
}
```
- Events (if button pressed, action happens)
```jsx
<button onClick={onClick}>{text}</button>
...
Button.propTypes = {
    onClick: PropTypes.func.isRequired
} 
```
- Lists
```jsx
const tasks = [
    {
        id: 1,
        text: 'Post YouTube video',
        day: 'Feb 5th at 2:30pm',
        reminder: true
    },
    ...
]
...
<>{tasks.map(
        (task) => (
        <h3 key={task.id}>{task.text}</h3>
        ))}
    </>
```
Notes: Each item in a list shoul have a unique ```key```.
- Use states
```jsx
import { useState } from "react"
...
const [tasks, setTasks] = useState([
    {
        id: 1,
        text: 'Post YouTube video',
        day: 'Feb 5th at 2:30pm',
        reminder: true
    },
    {
        id: 2,
        text: 'Develop Website',
        day: 'Feb 6th at 3:30pm',
        reminder: true
    },
    {
        id: 3,
        text: 'Drive NPCs in GTA to hospital',
        day: 'Feb 7th at 4:30pm',
        reminder: false
    }
])
```
- ```setTask``` is used to update the state with the new tasks.
```jsx
setTasks(tasks.filter((task)=>task.id !==id))
```
- Calling a passed-as-parameter function
```jsx
const <component_name> = ({func}) => {
  return (
    <div>
        <FaTimes onClick={func}/>
    </div>
  )
}
```
- Calling a passed-as-parameter function with parameters
```jsx
const <component_name> = ({func}) => {
  return (
    <div>
       <FaTimes onClick={() => func(task.id)}/>
    </div>
  )
}
```

- String interpolation
```jsx
const <component_name> = ({func}) => {
  return (
    <div className={`someClass ${<boolean_expression ? 'someOtherClass' : ''}`} >...</div>
  )
}
```
Note: Pay attention to ` and '

- Replace an item in a list based on a condition
```jsx
list.map((item) => <boolean_expression> ? {...item, parameter: nweValue} : item)
    )
```
- If-else in JavaScript
```jsx
if (condition) {
  instr1
} else {
  instr2
}
```
or

```jsx
condition ? instr1 : instr2
```

- If in JavaScript
```jsx
if (condition) {
  instr1
}
```
or

```jsx
condition && instr1
```

***

## API
- API calls should be studied more, but that is outside the scope of this course. However:
  - One must use ```useEffect```:
  ```jsx
  import { useState, useEffect } from "react"
  ...
  useEffect(() => {
    const getTasks = async () => {
      const tasksFromServer = await fetchTasks()
      setTasks(tasksFromServer)
    }

    getTasks()
  }, [])

  const fetchTasks = async () => {
    const res = await fetch('http://localhost:8080/tasks')
    const data = await res.json()

    return data
  }

  const fetchTask = async (id) => {
    const res = await fetch(`http://localhost:8080/tasks/${id}`)
    const data = await res.json()

    return data
  }
  
  const addTask = async (task) => {
    const res = await fetch(`http://localhost:8080/tasks/`, {
      method: "POST",
      headers: {
        'Content-type': 'application/json'
      },
      body: JSON.stringify(task)
    })

    const data = await res.json()

    setTasks([...tasks, data])
    }
  ```
  ```jsx
  const deleteTask = async (id) => {
    await fetch(`http://localhost:8080/tasks/${id}`, {method: 'DELETE'})

    setTasks(tasks.filter((task)=>task.id !==id))
  }

  const toggleReminder = async (id) => {
    const taskToToggle = await fetchTask(id)
    const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

  const res = await fetch(`http://localhost:8080/tasks/${id}`, {
    method: 'PUT',
    headers: {
      'Content-type': 'application.json'
    },
    body: JSON.stringify(updTask)
  })

  const data = await res.json()
  
  setTasks(tasks.map((task) => 
    task.id === id ? {...task, reminder: data.reminder} : task)
    )
  }
  ```

  ## Routes
- Import ```import {BrowserRouter as Router, Route, Routes} from "react-router-dom"```
- A route looks as follows:
```jsx
<Route path='/about' element={<About />} />
```
- A Route must be inside a Routes component
```jsx
<Routes>
  <Route ... />
  <Route ... />
</Routes>
```
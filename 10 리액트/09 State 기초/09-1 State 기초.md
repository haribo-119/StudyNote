
main.jsx 공통
```jsx hl:4,8
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
  
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

## 1. const [count,setConut] = useState(0) 기초
App.jsx
```jsx
import './App.css'
import {useState} from 'react'

function App() {
  // 1) const [count,setConut] = useState(0) 기초
  const [count,setConut] = useState(0)
  console.log(useState(0))
  
  return (
    <>
      <h2>Count: {count}</h2>
      <button onClick={() => setConut(count+1)}>
        +
      </button>
      <button onClick={() => setConut(count-1)}>
        -
      </button>
    </>
  )
}
export default App
```

결과
<img src="Pasted image 20250903234043.png" width=100/>


---

## 2. State의 true, false 값 사용
App.jsx
```jsx
import './App.css'
import {useState} from 'react'

function App() {
  // 2) State의 true, false 값 사용
  const [isPinned, setPinned] = useState(false)
  const togglePinned = () => {
    setPinned(!isPinned)
  }
  return (
    <>
      <button onClick={togglePinned}>
         {isPinned && '📌'} Pinn This!
      </button>
    </>
  )
}
export default App
```

결과
<img src="Pasted image 20250903234822.png" width=200/>


---

## 3. state를 사용한  Todo List
```jsx
import './App.css'
import {useState} from 'react'

function App() {
  // 3) state를 사용한  Todo List
  const [todos, setTodos] = useState(
    ['Learn React','Build a project']
  )
  const [newTodo, setNewTodo] = useState('')
  const addTodo = (newTodo) => {
    setTodos([...todos, newTodo])
    setNewTodo('')
  }

  const deleteTodo = (index) => {
    setTodos(
      todos.filter(
        (_,i) => i !== index)
      );
  }

  return (
    <>
      <h3>Todo List</h3>
      <ul>
        {todos.map((todo,index) =>(
          <li key = {index}>
            {todo}
            <button onClick={
              () => deleteTodo(index)}>
                Delete
              </button>
          </li>
        ))}
      </ul>
      <p>Typing: {newTodo}</p>
      <input type='text' value={newTodo}
        onChange={
          (e) => setNewTodo(e.target.value)}/>
      <button onClick={()=> addTodo(newTodo)}>
          Add New Task
      </button>    
    </>
  )
}
export default App
```

결과
<img src="Pasted image 20250904000944.png" width=350/>


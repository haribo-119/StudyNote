
main.jsx 공통
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
createRoot(document.getElementById('root')).render(
 /*  왜 렌더링이 두 번 일어날까?
  *  <StrictMode>는 함수 컴포넌트의 body(즉, 함수 전체)를 두 번 실행해서
  *  side effect(부수 효과)가 안전하게 작성되었는지 확인한다
  *  실제로는 렌더링이 두 번 일어나지만, 이는 개발 환경에서만 그렇고,
  *  프로덕션(배포)에서는 한 번만 렌더링된다
  */
  //<StrictMode>
    <App />
  //</StrictMode>,
)
```

## 1.부모 컴포넌트의 state가 props로 자식 컴포넌트에 전달
App.jsx
```jsx error:11,31
import './App.css'
import {useState} from 'react'
import Profile from './Profile'
  
function App(){

  /* 1) Profile.jsx(자식) 컴포넌트만 호출시, 자식 컴포넌트만 리렌더링
   * 2) App.jsx(부모) 에서 컴포넌트 호출시, 부모, 자식 컴포넌트 리렌더링
   */
  const users = ['Alice','Bob','Clark']
  const [user, setUser] = useState(users[0])
  const [status, setStatus] = useState(true)

  console.log('App Rendered')

  return(
    <>
      <h2>User Profile</h2>
      <button onClick={()=> setStatus(!status)}>
        Toggle Status
      </button>
      <button onClick={
        () => setUser(
          users[(users.indexOf(user)+1) % users.length]
        )}>
        Switch User
      </button>
      <p>
        <b>{user}</b> | {status ? 'Active':'Inactive'}
      </p>
      <Profile name={user}/>
    </>
  )
}
export default App
```

Profile.jsx
```jsx error:3,9
import {useState} from 'react'

function Profile({name}){
    const [status, setStatus] = useState('Available')
    console.log('Profile Rendered')

    return (
        <div className="user-profile">
            <h3>Name: {name}</h3>
            <p>Status : {status}</p>
            <button onClick={()=> setStatus('Away')}>
                Set Away
            </button>
            <button onClick={()=> setStatus('Available')}>
                Set Available
            </button>
        </div>
    )
}
export default Profile
```

 결과
 <img src="Pasted image 20250906152638.png" width=250/>


---

##  2.state(상태) 끌어올리기
 - 여러 컴포넌트들이 state를 공유해야 할 때, 이들을 부모 컴포넌트로 끌어올린 다음 변경 함수를 props로 전달하는 패턴을 상태 끌어올리기라고 한다
 
App.jsx

```jsx error:4,25-29 hl:5,30-33
import './App.css'
import {useState} from 'react'

import TempInput from './TempInput'
import UnitSelector from './UnitSelector'


function App(){
  /*
   * state(상태) 끌어올리기
   */
  const [temperature, setTemperature] = useState("")
  const [unit, setUnit] = useState("Celsius")
  const convertedTemp = unit === "Celsius"
    ? (temperature * 9/5 + 32).toFixed(1)
    : ((temperature - 32) * 5/9).toFixed(1)

  return (
    <div>
      <h2>Temperature Converter</h2>
      <p>
        Converted: {temperature ? convertedTemp : "--"}
        {unit === "Celsius" ? "°F" : "°C"}
      </p>
      <TempInput
        value={temperature}
        unit={unit}
        onChange={setTemperature}
      />
      <UnitSelector
        unit={unit}
        onUnitChange={setUnit}
      />
    </div>
  )
}
export default App
```


Templnput.jsx
```jsx
  /*
   * state(상태) 끌어올리기 
   */
import React from 'react'

const TempInput = (
  { value, unit, onChange }
) => {
  return (
    <div className='temp-input'>
      <input
        type="number"
        value={value}
        onChange={
          e => onChange(e.target.value)
        }
        placeholder={
          `In ${unit}`
        }
      />
      <span> {unit}</span>
    </div>
  )
}

export default TempInput
```

UnitSelector.jsx
```jsx
  /*
   * state(상태) 끌어올리기 
   */
import React from 'react'

const UnitSelector = ({ unit, onUnitChange }) => {
  return (
    <div className='unit-selector'>
      <label>
        <input
          type="radio"
          value="Celsius"
          checked={unit === "Celsius"}
          onChange={
            e => onUnitChange(e.target.value)}
        />
        Celsius
      </label>
      <label>
        <input
          type="radio"
          value="Fahrenheit"
          checked={unit === "Fahrenheit"}
          onChange={
            e => onUnitChange(e.target.value)}
        />
        Fahrenheit
      </label>
    </div>
  )
}

export default UnitSelector
```

결과
<img src="Pasted image 20250906164503.png" width=220/>


---

## 3.Form1

App.jsx
```jsx hl:3,8
import './App.css'
import {useState} from 'react'
import Form from './Form' 

function App(){

  return(
    <Form/>
  )  
}

export default App
```

Form.jsx
```jsx error:26,32,37
import './App.css'
import { useState } from 'react'

function Form() {

 /*
  * 1) Form 
  */
  const [username, setUsername] = useState('')
  const [isSubscribed, setSubscribed]
   = useState(false)
  const [role, setRole] = useState('user')
  const roles = ['user', 'admin', 'guest']

  return (
    <form>
	    <p>
        Name: {username}
        {isSubscribed && ' (Subscribed)'}
      </p>
      <p>Role: {role}</p>
      <input
        type="text" placeholder="Username"
        value={username}
        onChange={
          (e) => setUsername(e.target.value)
        }/>
      <input
        type="checkbox"
        checked={isSubscribed}
        onChange={
          (e) => setSubscribed(e.target.checked)
          }/>
      <label>Subscribe</label>

      <select value={role} onChange={
        (e) => setRole(e.target.value)}>
        {roles.map((r) => (
          <option key={r} value={r}>
            {r}
          </option>
        ))}
      </select>
    </form>
  )
}
export default Form
```

결과
<img src="Pasted image 20250906170443.png" width=200/>


---

## 4.Form2

App.jsx
```jsx hl:3,8
import './App.css'
import {useState} from 'react'
import Form from './Form' 

function App(){

  return(
    <Form/>
  )  
}

export default App
```

 Form.jsx
```jsx error:15-21,36,44,51
import './App.css'
import { useState } from 'react'

function Form() {
 /*
  * 2) Form 
  */
const [formData, setFormData] = useState({
    username: '',
    isSubscribed: false,
    role: 'user'
  })
  const roles = ['user', 'admin', 'guest']

  const handleChange = (e) => {
    const { name, value, type, checked }
     = e.target
    setFormData({
      ...formData,
      [name]:  type === 'checkbox' ? checked : value
    })
  }

  return (
    <form>
      <p>
        Name: {formData.username}
        {formData.isSubscribed && ' (Subscribed)'}
      </p>
      <p>Role: {formData.role}</p>
      <input
        type="text"
        name="username"
        placeholder="Username"
        value={formData.username}
        onChange={handleChange}
      />

      <label>
        <input
          type="checkbox"
          name="isSubscribed"
          checked={formData.isSubscribed}
          onChange={handleChange}
        />
        Subscribe
      </label>

      <select 
        name="role" value={formData.role}
        onChange={handleChange}>
        {roles.map((r) => (
          <option key={r} value={r}>
            {r}
          </option>
        ))}
      </select>
    </form>
  )
}
export default Form
```

결과
<img src="Pasted image 20250906182527.png" width=200/>


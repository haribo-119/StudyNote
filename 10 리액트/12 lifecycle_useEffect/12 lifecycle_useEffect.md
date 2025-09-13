
## 1. 생명주기

main.jsx 
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  // <StrictMode>
    <App />
  // </StrictMode>,
)
```

App.jsx
```jsx
import './App.css'
// 1) 생명주기
import ClassComp from './ClassComp'
import FuncComp from './FuncComp'
import { useState } from 'react'

const App = () => {
  //1) 생명주기 
  const [selected, setSelected] = useState('')
  return (
    <>
      {['', 'ClassComp', 'FuncComp'].map((option) => (
        <label key={option}>
          <input
            type="radio" value={option}
            checked={selected === option}
            onChange={(e) => setSelected(e.target.value)}/>
          {option || 'None'}
        </label>
      ))}
      {selected && (selected === 'ClassComp' ? <ClassComp /> : <FuncComp />)}
    </>
  )
}
export default App
```

ClassComp.jsx
```jsx error:1,3
import { Component } from 'react'

class ClassComp extends Component {

//생성자(constructor) - 컴포넌트가 처음 만들어질때 실행
  constructor(props) {
    super(props) // 부모 클래스의 생성자를 호출
    this.state = { count: 0 } // count라는 상태값을 0으로 초기화
  }

/* 생명주기 메서드
 * componentDidMount(), componentDidUpdate(), componentWillUnmount()
 */
  componentDidMount() { // 컴포넌트가 화면에 처음 나타날 때 실행
    console.log('1. Mounted')
  }

  componentDidUpdate() {// 컴포넌트가 업데이트(리렌더링)될 때 실행
    console.log('2. Updated')
  }

  componentWillUnmount() {// 컴포넌트가 화면에서 사라질 때 실행
    console.log('3. Unmounted')
  }

  handleIncrement = () => {
    this.setState(prevState => (
      { count: prevState.count + 1 }
    ))
  }

  render() {
    console.log('-- Rendering --')
    return (
      <div>
        <h2>Class Component</h2>
        <h3>Count: {this.state.count}</h3>
        <button onClick={this.handleIncrement}>
          Increase
        </button>
      </div>
    )
  }
}

export default ClassComp
```

- `{jsx icon}import { Component } from 'react'`여기서 MyComponent는 Component를 상속받아서
React의 모든 기능(생명주기, state 등)을 사용할 수 있게 된다

super(props)를 사용하는 이유
```jsx
  constructor(props) {
    super(props) // 부모 클래스의 생성자를 호출
    this.state = { count: 0 } // count라는 상태값을 0으로 초기화
  }
```
- constructor에서 반드시 super(props)를 먼저 호출해야 this를 사용할 수 있다
- this는 현재 컴포넌트(ClassComp.jsx)
<br>
####  ※ 함수형 컴포넌트와의 차이
- 함수형 컴포넌트는 그냥 함수로 만들지만,
- 클래스형 컴포넌트는 반드시 Component를 상속받아야 한다
<br>

FuncComp.jsx
```jsx
import { useState, useEffect } from 'react'

const FuncComp = () => {
  const [count, setCount] = useState(0)

  useEffect(() => {
    console.log('1. Mounted')
    return () => {
      console.log('3. Unmounted')
    }
  }, [])

  useEffect(() => {
    console.log('1. Mounted / 2. Updated')
  }, [count])

  const handleIncrement = () => {
    setCount(prevCount => prevCount + 1)
  }

  console.log('-- Rendering --')

  return (
    <div>
      <h2>Function Component</h2>
      <h3>Count: {count}</h3>
      <button onClick={handleIncrement}>
        Increase
      </button>
    </div>
  )
}

export default FuncComp
```

#### useEffect란?
- React 함수형 컴포넌트에서 "부수 효과(side effect)"를 처리하는 훅(Hook)
- 예전 클래스형 컴포넌트의
- componentDidMount, componentDidUpdate, componentWillUnmount
- 같은 생명주기 함수 역할을 함수형 컴포넌트에서 할 수 있게 한다
#### useEffect 사용예제
- 데이터 불러오기(ajax, fetch)
- 타이머 설정/해제
- 이벤트 리스너 등록/해제
- 콘솔 출력, 외부 라이브러리 연동 등

결과
<img src="Pasted image 20250913163904.png" width=430/>


---
## 2. useEffect

main.jsx
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  // <StrictMode>
    <App />
  // </StrictMode>,
)
```

App.jsx
```jsx
import './App.css'

// 2) useEffect
import {useState, useEffect} from 'react'

const App = () => {
  // 2) useEffect
  const [count1, setCount1] = useState(0)
  const [count2, setCount2] = useState(0)

  const handleIncrease = (setter) => { setter((prev) => prev + 1) }

  useEffect(() => {
    console.log(`C1: ${count1}, C2: ${count2}`)
  }, [count1]) // console.log에는 count1이 클릭되어야 이벤트가 발생됨
  // useEffect의 [] 배열이 비어있으면 모든 이벤트에 발생 됨

  return (
    <div>
      <h2>Count1: {count1}</h2>
      <button onClick={() => handleIncrease(setCount1)}>Count1++</button>

      <h2>Count2: {count2}</h2>
      <button onClick={() => handleIncrease(setCount2)}>Count2++</button>
    </div>
  )

}
export default App

```

결과
<img src="Pasted image 20250913164646.png" width=270/>

---
## 3. useEffect - json 데이터 호출

main.jsx
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import JsonUseEffect from './JsonUseEffect.jsx'

createRoot(document.getElementById('root')).render(
  // <StrictMode>
    // <App />
    <JsonUseEffect />
    // <TimerStart/>
  // </StrictMode>,
)
```

JsonUseEffect.jsx
```jsx
import './App.css'
import { useState, useEffect} from 'react'


// 3)  useEffect - json 데이터 호출
const JsonUseEffect = () => {
    const [books, setBooks] = useState([])
    const [loading, setLoading] = useState(true)
  
    useEffect(() => {
      const fetchBooks = async () => {
        try {
          const response = await fetch('/data/books.json') // 파일 경로에  백엑드에서 url이 들어감
          const data = await response.json()
          setBooks(data)
        } catch (error) {
          console.error('Failed to fetch books:', error)
        } finally {
          setLoading(false)
        }
      }
  
      fetchBooks()
    }, []) // [] 배열은 컴포넌트가 마운트 되는 시점에 딱 한번만 fetchBooks() 실행
  
    if (loading) return <p>Loading...</p>
  
    return (
      <div>
        <h2>Book List</h2>
        <ul>
          {books.map((book) => (
            <li key={book.id}>
              <strong>{book.title}</strong> by {book.author}
            </li>
          ))}
        </ul>
      </div>
    )
  }

export default JsonUseEffect

```

/data/books.json
```json
[
    {
      "id": 1,
      "title": "React Basics",
      "author": "John Doe"
    },
    {
      "id": 2,
      "title": "Advanced React",
      "author": "Jane Smith"
    },
    {
      "id": 3,
      "title": "JavaScript Essentials",
      "author": "Alan Turing"
    },
    {
      "id": 4,
      "title": "CSS Mastery",
      "author": "Rachel Green"
    }
  ]
```

결과
<img src="Pasted image 20250913175223.png" width=300/>

---

## 4. useEffect 언마운트

main.jsx
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import JsonUseEffect from './JsonUseEffect.jsx'
import TimerStart from './TimerStart.jsx'

createRoot(document.getElementById('root')).render(
  // <StrictMode>
    // <App />
    // <JsonUseEffect />
    <TimerStart/>
  // </StrictMode>,

)
```

TimerStart.jsx
```jsx
import './App.css'
import {useState} from 'react'
import Timer from './Timer'

// 4) useEffect 언마운트
const TimerStart = () => {
    const [showTimer, setShowTimer]
    = useState(false)

    return (
        <>
            <label>
                <input
                    type ="checkbox"
                    checked= {showTimer}
                    onChange = {
                        (e) => setShowTimer(
                            e.target.checked
                        )}/>
                Show Timer
            </label>
            {showTimer && <Timer/>}
        </>
    )
} 
export default TimerStart
```

Timer.jsx
```jsx
import {useState, useEffect} from 'react'

const Timer = () => {
    const [seconds, setSeconds] = useState(0)

    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds((prev) => prev +1)
        },1000)

    return () => {  // useEffect가 언마우트 될때 동작
        clearInterval(interval)
        console.log('Timer cleaned up')
    }

    },[]) // useEffect
    return <p>Timer : {seconds} seconds</p>
}
export default Timer

```

결과
<img src="Pasted image 20250913233651.png" width=180/>


- useRef는 DOM 요소에 직접 접근하거나, 변수처럼 값을 저장할 때 사용되는 훅(Hook)

main.jsx 공통
```jsx
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

## 1. useRef
 - useState와 useReducer는 값을 바꿀 때 화면도 렌더링되지만
 - ==useRef는 렌더링과 관계없이 변경 가능한 값==들을 가실 수 있다

App.jsx
```jsx
import './App.css'
import Counter from './Counter'
import {useRef} from 'react'


const App = () => (
  <>
    <Counter/> 
  </>
)

export default App
```

Counter.jsx 버전1
```jsx error:3-5 hl:12
import { useState, useRef } from 'react'

// 1) useRef
// useState와 useReducer는 값을 바꿀 때 화면도 렌더링되지만
// useRef는 렌더링과 관계없이 변경 가능한 값들을 가실 수 있다
function Counter() {
  const count1 = useRef(0)
  const [count2, setCount2] = useState(0)

  const incrementRef = () => {
    count1.current += 1
    {/* current는 useRef의 속성으로 저장소*/}
    console.log('Ref Count:', count1.current)
  }
  
  return (
    <>
      <h2>Counter Counter</h2>
      <p>Count 1: {count1.current}</p> 
      {/* count1.current - 현재 값을 나타냄  */}
      <p>Count 2: {count2}</p>
      <button onClick={incrementRef}>useRef</button>
      <button onClick={() => setCount2(c => c + 1)}>useState</button>
    </>
  )
}
export default Counter
```

결과
<img src="Pasted image 20250908214656.png" width=450/>


---

## 2. const refCount = useRef(0) 사용법

App.jsx
```jsx
import './App.css'
import Counter from './Counter'
import {useRef} from 'react'


const App = () => (
  <>
    <Counter/> 
  </>
)

export default App
```

Counter.jsx 버전2
```jsx error:3,7,15
import { useState, useRef } from 'react'

// 2) const refCount = useRef(0)
function Counter() {
    const [count1, setCount1] = useState(0)
    const [count2, setCount2] = useState(0)
    const refCount = useRef(0)
    const incrementRef = () => {
      refCount.current += 1
      console.log(
        'Ref Count:', refCount.current
      )
    }
    const syncCounts = () => {
      setCount1(refCount.current)
      setCount2(prev => prev + 1)
    }
    return (
      <>
        <h2>Counter Counter</h2>
        <p>Count 1: {count1}</p>
        <p>Count 2: {count2}</p>
        <button onClick={incrementRef}>
          useRef
        </button>
        <button onClick={syncCounts}>
          useState
        </button>
      </>
    )
  }  
export default Counter
```

결과
<img src="Pasted image 20250908214656.png" width=450/>

버전1 useRef 값이 리렌더링 때만 화면에 방영
버전2 setCount1(refCount.current)로 state에 복사해야 화면에 바로 반영

---

## 3. useRef 대신 변수에 저장했을때_1

App.jsx
```jsx
import './App.css'
import Counter1 from './Counter1'

const App = () => (

  <>
    {/* 3) useRef 대신 변수에 저장했을때, 저장되지 않음 */}
    <Counter1/>
    {/* <Counter1/>  */}
  </>
)
export default App
```

Counter1.jsx
```jsx hl:13,20,11,4
import { useState } from 'react'
// 3) useRef 대신 변수에 저장했을때
// countVar가 초기화됨, 리랜더링 될때마다 다시 실행됨
// let countVar = 0 전역변수를 사용할 경우,
// APP.jsx 에서 <Counter1/> , <Counter1/>  중복으로 두번 사용하게될때
// 전역변수로 한가지  countVar 전역 변수만을 참조함
function Counter1() {
  const [count1, setCount1] = useState(0)
  const [count2, setCount2] = useState(0)

  let countVar = 0 // 버튼 클릭시 리랜더링, 초기화됨
  const incrementVar = () => {
    countVar++
    console.log(
      'Var Count:', countVar
    )
  }

  const syncCounts = () => {
    setCount1(countVar)
    setCount2(prev => prev + 1)
  }
  return (
    <>
      <h2>Counter App</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <button onClick={incrementVar}>
        local var
      </button>
      <button onClick={syncCounts}>
        useState
      </button>
    </>
  )
}  
export default Counter1
```

결과
<img src="Pasted image 20250909014719.png" width=150/>
 - [local var] 버튼 클릭시 Count1이 개발자 환경 consol.log에서 1,2,3,4 증가하는걸로 보이나,
 - [useSate]버튼 클릭시 리렌더링 되어 consol.log가 1부터 시작된다
---

## 4. useRef 대신 변수에 저장했을때_2
App.jsx
```jsx hl:8,9
import './App.css'
import Counter from './Counter'
import Counter1 from './Counter1'

const App = () => (
  <>
    <Counter1/>
    <Counter1/>
  </>
)
export default App
```

Counter1.jsx
```jsx hl:13,20,11,4
import { useState } from 'react'
// 3) useRef 대신 변수에 저장했을때
// countVar가 초기화됨, 리랜더링 될때마다 다시 실행됨
 let countVar = 0 // 전역변수를 사용할 경우,
// APP.jsx 에서 <Counter1/> , <Counter1/>  중복으로 두번 사용하게될때
// 전역변수로 한가지  countVar 전역 변수만을 참조함
function Counter1() {
  const [count1, setCount1] = useState(0)
  const [count2, setCount2] = useState(0)

// let countVar = 0 // 버튼 클릭시 리랜더링, 초기화됨
  const incrementVar = () => {
    countVar++
    console.log(
      'Var Count:', countVar
    )
  }

  const syncCounts = () => {
    setCount1(countVar)
    setCount2(prev => prev + 1)
  }
  return (
    <>
      <h2>Counter App</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <button onClick={incrementVar}>
        local var
      </button>
      <button onClick={syncCounts}>
        useState
      </button>
    </>
  )
}  
export default Counter1
```

결과
<img src="Pasted image 20250909210249.png" width=400/>

 ① 파란박스에서 [local var] 3번 클릭후 [useState]버튼 클릭시 Count1 : 3 , Count2 : 1 값 출력
 ② 빨간박스에서 [useState] 버튼 클릭시 Count 1 : 3 값을 출력 
 ③ 지역 변수이고, 해당 값이 각 컴포넌트에서 공유됨 (useRef은 컴포넌트 마다 관리됨)

---

## 5. DOM을 제어하는 용도로 useRef
App.jsx
```jsx
import './App.css'
import {useRef} from 'react'

const App = () => {

  // 5) 버튼 클릭시 input창에 커서가 위치할 수 있도록 구현
  // useRef에는 DOM 요소를 담을 수도 있다
  const inputRef = useRef(null) // DOM 요소를 담을 때, null로 초기화
  const handleFocus = () => {
    console.log(inputRef.current) // <input> 태그가 담김
    inputRef.current.focus() // 커서를 포커스
  }

    return (
      <div>
        {/*ref 객체에 넣어준다,input 요소가 담기게 됨*/}
        <input ref={inputRef}      
        type="text" placeholder='Type...'/>
        <button onClick={handleFocus}>
          Focus Input
        </button>
      </div>
    )
  }
export default App
```

결과
<img src="Pasted image 20250909203604.png" width=600/>
위에는 버튼클릭시, focus를 input창에 위치하도록 한다
<br>
- useRef 객체의 current 값은 역시 해당 인풋의 DOM 객체인 것을 확인할 수 있다
- console창은 가상 DOM이 아니라 실제 DOM을 가르키고 있다
<br>
이런식으로 특정 요소를 포커스하거나 특정 요소를 스크롤하는 등, DOM을 제어하는 용도로 useRef가 사용될 수 있다



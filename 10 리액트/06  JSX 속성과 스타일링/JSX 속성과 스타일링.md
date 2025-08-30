
main.jsx는 App.02.jsx를 호출
```jsx title:main.jsx hl:4,8 
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App02 from './App02.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App02/>
  </StrictMode>,
)
```

App02.jsx

```jsx title:JSX_속성과_스타일
//JSX 속성과 스타일
import './App.css'

function App02(){
    // 1) HTML에서 사용하는 속성과 차이가 있다
    const disableInput = true
    return(
            <>
                <label
                    htmlFor="username"  // for
                >
                    Username:
                </label>
                <input
                    type = "text"
                    id = "username"
                className="input-field"        // class
                autoComplete="off"             // autocomplete
                maxLength={20}                 // maxlength
                spellCheck={true}              // spellCheck
                readOnly={false}               // readOnly
                tabIndex={0}                   // tabindex
                disabled={disableInput}
                placeholder={
                    disableInput ? "(DISABLED)" : "Enter your name"
                }
                >
                </input>
            </>
             )
}
export default App02;
```

결과
<img src="Pasted image 20250830182917.png" width=250 hight=200/>

---

main.jsx
```jsx title:main.jsx hl:4,8 
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App03 from './App03.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App03/>
  </StrictMode>,
)
```

App03.jsx

```jsx title:JSX_속성과_스타일
//JSX 속성과 스타일
import './App.css'
import logo from './assets/logo.jpg'

const logoAlt = 'twitter Logo'

console.log(logo)

function App03() {
    const divStyle = {
        backgroundColor : 'lightblue',
        margin:'12px',
        padding:'20px',
        borderRadius:'8px'
    }
    return(
            <>
            {/* 1) 이미지 출력 */}
             <img
                src={logo}
                alt={logoAlt}
                width={100}
                height={80}
                />
             {/* 2) 속성1 */}
             <span style={
                {
                  fontWeight:"bold",
                  fontStyle:"italic"
                }
             }>
                Bold & Italic
             </span>
            {/* 3) 속성2 */}
            <div style={divStyle}>
                DIV 1
            </div>
            <div
                style={{
                // spread 연산자란? (divStyle)의 모든 속성을 복사해서 가져온다
                // 만약 divStyle에 같은 속성이 있다면, 여기서 지정한 값이 덮어씁니다(우선 적용).
                  ...divStyle, // <- spread 연산자
                  backgroundColor : 'lightgreen',
                  color:'darkblue',
                  fontWeight:'bold',
                }}
            >
                DIV 2
            </div>
            </>
             )
}
export default App03;
```

결과
<img src="Pasted image 20250830184018.png" width=150 hight=150/>

---

main.jsx
```jsx title:main.jsx hl:4,8 
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App04 from './App04.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App04/>
  </StrictMode>,
)
```

App04.jsx
```JSX title:JSX_속성과_스타일
//JSX 속성과 스타일
import './App.css'

function App04() {
    const styleA = {
        color:'darkred',
        fontWeight :'bold',
    }
    const styleB = {
        color:'navy',
        textDecoration:'underline',
    }
    const isPrimary = true
    return(
        <>
            <div style={isPrimary ? styleA : styleB}>
                This text has dynamic styling.
            </div>
            <span
                style={{
                    fontSize : isPrimary ? '1.5em' : '1em',
                    opacity  : isPrimary ? 1 : 0.5
                }}
                >
                 So does this text.
                </span>
        </>
             )
}
export default App04;
```

결과
<img src="Pasted image 20250830184836.png" width=200 hight=200/>

---

main.jsx
```jsx title:main.jsx hl:4,8 
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App05 from './App05.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App05/>
  </StrictMode>,
)
```

App05.jsx
```jsx title:JSX_속성과_스타일
//JSX 속성과 스타일 (css 외부 파일 충돌 방지)
import './App.css'
import ButtonA from './ButtonA'
import ButtonB from './ButtonB'

function App5() {
    // 1) ButtonA, ButtonB의 컴포넌트는 전역으로 사용
    //    import './ButtonA.css' 와 import './ButtonB.css'로 구분했지만,
    //    className="button"이 같고, 컴포넌트가 전역으로 사용되어 .button{ } css를 모두 참조

    // 2) 위와 같은 문제로    ButtonA.css -> ButtonA.module.css 파일명 변경
    //    import styles from './ButtonA.module.css';
    //    <button className={styles.button}> 변수로 참조해서 적용되게 변경
    return(
        <>
            <ButtonA/>
            <ButtonB/>
        </>
    )
}  
export default App5;
```

ButtonA.jsx
```jsx title:ButtonA
import React from 'react';
// import './ButtonA.css';
import styles from './ButtonA.module.css';
  
function ButtonA() { 
    console.log(styles)  
    return (
        // <button className="button">
        <button className={styles.button}>
            Button A
        </button>
    )
}
export default ButtonA;
```

ButtonA.module.css
```css title:ButtonA_css
.button{
    background-color: skyblue;
    padding:10px;
    font-size:16px;
}
.slider {padding: 0;}
#topmenu {margin: 0;}
```

ButtonB.jsx
```jsx title:ButtonB
import React from 'react';
// import './ButtonB.css';
import styles from './ButtonB.module.css';

function ButtonB() {
    console.log(styles)
    return (
        // <button className="button">
        <button className={styles.button}>
            Button B
        </button>
    )
}
export default ButtonB;
```

ButtonB.module.css
```css title:ButtonB_css
.button{
    border : 2px solid red;
    margin : 15px;
    text-transform: uppercase;
}
```

결과
<img src="Pasted image 20250830185903.png" width=200 hight=200/>

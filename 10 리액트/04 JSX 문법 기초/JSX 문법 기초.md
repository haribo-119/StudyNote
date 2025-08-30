
main.jsx는 App.jsx를 호출
```jsx title:main.jsx hl:4,8 
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


```Jsx title:App.jsx_문자열 hl:2,4,13-14

const element1 =<h2>Hello, world!</h2>

const element2 = (
  <ul>
    <li>A</li>
  </ul>
)

function ElementExpressions(){
  return(
    <section>
      {element1}
      {element2}
    </section>
  )
}
function App(){
  return(
    <>
	    <ElementExpressions />
    </>
  )
}
export default App;
```

결과
<img src="Pasted image 20250830001328.png" width=150 hight=150/>


```jsx title:App.jsx_문자열 error:5,13 hl:14
function BasicExpressions(){

  const name = "John"
  const age = 25;
  const isAdmin = true;
  
  return (
    <div>
      <p>Name : {name}</p>
      <p>Age next year : {age + 1}</p>
      <p>{name + "'s Profile"}</p>
      <p>{`${name} is ${age} years old`}</p>
      <p>Admin status : {String(isAdmin)}</p>{/*String으로 변환*/}
      {/*문자가 boolean타입일 경우 String으로 명시적 변환이 필요*/}
    </div>
  );
}

function App(){
  return(
    <>
    <BasicExpressions />
    </>
  )
}
export default App;
```


결과
<img src="Pasted image 20250830002911.png" width=150 hight=150/>


```jsx title:App.jsx_배열
function ObjectArrayExpressions(){

  const user = {
    name : "Jane",
    email : "jane@example.com"
  };

  const colors = ["red","blue","green"];
  const numbers =[1,2,3,4,5];

  return (
    <div>
      <p>User : {user.name}({user.email})</p>
      <p>First color : {colors[0]}</p>
      <p>Color count : {colors.length}</p>
      
      <p>Doubleds : {
        numbers.map(n => n*2).join(",")
        }</p>

       <p>Evens: {
         numbers.filter(n => n % 2 === 0).join(",")
        }</p>
    </div>

  );
}

function App(){
  return(
    <>
    <ObjectArrayExpressions />
    </>
  )
}

export default App;
```

결과
<img src="Pasted image 20250830004716.png" width=190 hight=190/>

```jsx title:App.jsx_함수형형
function FunctionExpressions(){

  const getGreeting = (name) => `Hello, ${name}!`;
  const formatDate = (date) => {
    return new Date(date).toLocaleDateString();
  };

  const calculateTotal = (items) => {
    return items.reduce((sum,item) => sum + item.price,0);
  };

  const items = [{id:1, price:10},{id:2, price:20}];

  return(
    <div>
      <p>{getGreeting("Alice")}</p>
      <p>Today : {formatDate(new Date())}</p>
      <p>Total : ${calculateTotal(items)}</p>
      <p>Good {(()=>{
        const hours = new Date().getHours();
        return hours < 12 ? "moring!" : "afternooon!";
      })()}</p>
    </div>
  );
}

function App(){
  return(
    <>
    <FunctionExpressions />
    </>
  )
}
export default App;
```

결과
<img src="Pasted image 20250830004643.png" width=150 hight=150/>

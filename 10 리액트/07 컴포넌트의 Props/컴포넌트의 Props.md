## 1. Props
- Props는 Properties(속성)의 줄임말
- React에서 컴포넌트에 값을 전달할 때 사용하는 방법
- 마치 함수에 파라미터를 넘기는 것과 유사함

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

`{jsx icon}App.css` 공통( 생략)

App.jsx
```jsx title:변경전
import './App.css'

function App(){
// 2-1) 변경전
   return(
    <>
      <InfoCard
        title="Props in React"
        content="Props pass data one component to another"
        author="Alice"
      />
      <InfoCard
        title="React Composition"
        content="Composision makes your components more reusable"
      />
    </>
   )
}
export default App
```

결과
<img src="Pasted image 20250831151723.png" width=180/>

App.jsx
```jsx title:변경후 hl:2
import './App.css'
import InfoCard from './InfoCard';

// 2-2) 변경후
const cardData1 = {
  title : "Props in React",
  content : "Props pass data from one component to another.",
  author:"Alice"
};

const cardData2 = {
  title : "React Composition",
  content : "composition makes your components more reuasble"
};

function App(){
  // 2-2)
  return(
    <>
      <InfoCard {...cardData1}/>
      <InfoCard {...cardData2}/>
    </>
  )
}
export default App
```

InfoCard.jsx
```jsx
import style from './Card.module.css'
// 1-1) 개선전
const InfoCard = (props) => (
    <div className={style.card}>
        <h2>{props.title}</h2>
        <p>{props.content}</p>
        <p>Author : {props.author}</p>
    </div>
)
export default InfoCard
```

Card.module.css 부분 공통
```css
.card{
    width:224px;
    margin:12px auto;
    padding:24px;
    border :1px solid #ddd;
    border-radius : 8px;
    box-shadow:0 4px 8px rgba(0,0,0,0.1);
}
.card h2 {
    margin: 0 0 8px;
    font-size:1.33rem;
    color:#333;
}
.crad p{
    margin: 2px 0;
}  
.crad p:last-child{
    font-size:0.92rem;
    color:#888;
}
```

결과
<img src="Pasted image 20250831155103.png" width=180/>

---
## 2. 배열
App.jsx
```jsx title:배열 error:6,12
import './App.css'
import InfoCard from './InfoCard';
// 2-3) 배열로 변경
const cards = [
{
  idx : 1, //배열이나 List를 렌더링 할때, index값이 필요
  title : "Props in React",
  content : "Props pass data from one component to another.",
  author:"Alice"
},
{
  idx :2, //배열이나 List를 렌더링 할때, index값이 필요
  title : "React Composition",
  content : "composition makes your components more reuasble"
}]

function App(){
  // 2-3) 배열 호출
  return (
    <>
    {cards.map(cardData =>(
      <InfoCard key={cardData.idx} {...cardData}/>
    ))}
    </>
  )
}
export default App
```

InfoCard.jsx
```jsx title:1-2)_개선후 error:2,12,22
import style from './Card.module.css'
// 1-1) 개선전
/*
const InfoCard = (props) => (
    <div className={style.card}>
        <h2>{props.title}</h2>
        <p>{props.content}</p>
        <p>Author : {props.author}</p>
    </div>
) 
*/
// 1-2) 개선후
/*
const InfoCard = ({title,content,author}) => (
    <div className={style.card}>
        <h2>{title}</h2>
        <p>{content}</p>
        <p>Author : {author}</p>
    </div>
)
*/
// 1-3) 개선후
const InfoCard =({
		{/*변수값의 default*/}
        title="(No title)",
        content,
        author="Anonymous"
    }) =>(    
    <div className={style.card}>
        <h2>{title}</h2>
        <p>{content}</p>
        <p>Author : {author}</p>
    </div>)
export default InfoCard
```

결과
<img src="Pasted image 20250831160240.png" width=180/>

---
### 3. formatPrice 전달 값
App.jsx
```jsx hl:14
import './App.css'
// 3-1) ProductCard
import ProductCard from './ProductCard';

function App(){
  // 3-1) ProductCard.jsx
  const product = {
    name:"Laptop",
    price:123.4567
  };
  return (
    <ProductCard
    {...product}
    formatPrice={(p) => `$${p.toFixed(2)}`}
    />
  )
}
export default App
```

ProductCard.jsx
```jsx hl:4,6-7,12
import style from './Card.module.css'

const ProductCard = ({
    name, price, formatPrice
}) => {
    const displayedPrice
    = formatPrice(price)
    
    return(
        <div className={style.card}>
            <h2>{name}</h2>
            <p>Price : {displayedPrice}</p>
        </div>
    )
}  
export default ProductCard
```

결과
<img src="Pasted image 20250831160941.png" width=180/>


---
### 4. children 내부 요소
App.jsx
```jsx error:9,13-15,19-20
import './App.css'
import CardLayout from './CardLayout';

function App(){
  // 4-1) CardLayout.jsx (children)
  return(
      <div>
        <CardLayout title="About">
          <p>Props of Components</p>
        </CardLayout>
        <CardLayout title="Details">
          <ul>
            <li>Frature A</li>
            <li>Frature B</li>
            <li>Frature C</li>
          </ul>
        </CardLayout>
        <CardLayout title="Contact">
          <p>Email : example@example.com</p>
          <p>Phone : 123-456-7890</p>
        </CardLayout>
      </div>
  )
}
export default App
```

CardLayout.jsx
```jsx error:4,11
import styles from './Card.module.css'
// 4-1) CardLayout.jsx (children)
const CardLayout = ({
    title, children // 내부 요소들인 들어옴
}) => {
    console.log(children)
    return(
    <div className={styles.card}>
        <h2>{title}</h2>
        <div className="content">
            {children}
        </div>
    </div>
    )
}
export default CardLayout;
```

결과
<img src="Pasted image 20250831161553.png" width=200/>

---
### 5. 고차 컴포넌트
App.jsx
```jsx
import './App.css'
// 5-1) 고차 컴포넌트
import withConditionalCard from './withConditionalCard';
import SimpleCard from './SimpleCard';

const ConditionalSimpleCard = withConditionalCard(SimpleCard)

function App(){
  // 5-1)
  return(
    <>
      <ConditionalSimpleCard
        title="Active Card"
        content="This card is active"
        disabled={false}
        />
      <ConditionalSimpleCard
        title="Disabled Card"
        content="This card is disabled"
        disabled={true}
      />
   </>
  )
}
export default App
```

simpleCard.jsx
```jsx
// 5-1) 고차 컴포넌트
function SimpleCard({title,content}){
    return (
        <div>
            <h2>{title}</h2>
            <p>{content}</p>
        </div>
    )
}  
export default SimpleCard;
```

withConditionalCard.jsx 
```jsx title:비활성화_표시 hl:15
import styles from './Card.module.css'
// 5-1) 고차 컴포넌트
function withConditionalCard(WrappedComp) {
    return function ConditionalCard({
        disabled, ...props
    }) {
        const cardStyle = {
            opacity : disabled ? 0.5 : 1,
        }
        return (
            <div
                className={styles.card}
                style={cardStyle}
            >
                <WrappedComp {...props}/> {/*<SimpleCard/> 출력*/} 
            </div>
        )
    }
}  
export default withConditionalCard
```
`{jsx icon}...props`는 title,content 값을 가져옴 

결과
<img src="Pasted image 20250831162125.png" width=180/>


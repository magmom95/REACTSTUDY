![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=300&section=header&text=리액트를%20다루는기술%20&fontSize=90&animation=fadeIn&fontAlignY=38&desc=%20이성규&descAlignY=65&descAlign=90)

* [x] 1장 : 리액트시작
* [x] 2장 : JSX
* [x] 3장 : 컴포넌트

# 1장 React를 쓰는 이유와 특징 그리고 Virtual Dom!

- React는 페이스북에서 개발한 Javascript 라이브러리이며 오직! View만 신경쓰는 라이브러리

- MVC패턴은 Model-View-Controller로 이루어져 있는 아키텍쳐

- 사용자가 컨트롤러를 통해 원하는 데이터를 요청하면 모델은 사용자가 원하는 데이터를 가져와서 뷰에게 전달해주면 뷰는 이 데이터를 잘 조리해서 사용자에게 보여주는 형식
<br>
<br>

![image](https://user-images.githubusercontent.com/64140544/143579298-cd5b5efe-a7a2-48f7-8769-8bc5b192d21f.png)

<br>
<br>
그렇다면 React는 왜 오직 View만 신경쓰는 구조로 만들어진 이유는?
<br>
<br>

![d9900472ea4af1a9e043e621e286d1ba](https://user-images.githubusercontent.com/64140544/143580169-5d75ef78-89c2-4914-b182-1d91d0a62288.gif)

<br>

- 페이스북은 많은 사용자를 보유하고 있고 사용자에게 좋은 경험을 선사하기 위해 고민

- 이 페이지들을 어떻게하면 최대한의 성능으로 사용자에게 제공할 수 있을까? 이런 고민을 하다 기존에 뷰를 그냥 날려버리고 처음부터 새로 렌더링하는 방식의 아이디어를 고안

- 실제로 그냥 뷰를 날려버리고 새로 렌더링 하게 된다면 매우 느려지고 CPU, 메모리 점유율도 크게 증가하지만 페이스북에서는 이런 방식을 토대로 최대한 성능을 아끼고, 편안한 사용자 경험을 제공하기 위해 React를 개발

그렇다면 페이스북은 어떻게 기존의 뷰를 날려버리고 새로 렌더링 하는 방식을 기반으로 좋은 성능을 뽑아낼 수 있었던 이유

- React의 Virtual DOM으로 인하여 가능

Dom이란?

- DOM은 Document Object Model의 약자로 문서 구조를 객체로 표현하는 방법
- 우리가 HTML를 사용해서 문서를 만들게 되면 아래와 같은 형태로 DOM에 저장
- DOM은 트리형태로 구성되어 있어서 특정 노드를 찾거나 제거하거나 혹은 원하는 곳에 추가
- 이러한 형태를 가지고 있기 때문에 Javascript 에서 실제 DOM에 접근하여 수정이 가능

<br>
<br>

![image](https://user-images.githubusercontent.com/64140544/143580395-487c7f36-aaf3-4843-a92d-613197c64d5b.png)
<br>

- Virtual DOM은 말 그대로 가상의 DOM React는 기존의 뷰를 날려버리고 새로 렌더링 한다고 했는데 이 방식이 Virtual DOM을 통해 이루어짐

<br>
<br>

![image](https://user-images.githubusercontent.com/64140544/143580718-6a022de0-fcc7-4ce4-8978-9497623b96c6.png)
<br>

실제로 작동하는 방식은

- 데이터가 변경되면 변경된 요소를 포함하여 똑같은 형태로 Virtual DOM에 저장
- Virtual DOM은 DOM과 똑같은 형태이기 때문에 서로의 상태를 확인
- 이때 변경된 부분이 있다면 Virtual DOM의 변경된 부분을 실제 DOM에 업데이트

즉, 초기 렌더링(DOM에 저장) → 데이터 업데이트(Virtual DOM에 저장) → 비교(기존의 DOM과 Virtual DOM을 비교) → 업데이트(비교했을 때 달라진 부분만 실제 DOM에 업데이트)

- 이러한 Virtual DOM 사용 방식으로 실제로 뷰를 전부다 날려버리는 것이 아닌 특정 부분이 업데이트 되었을때 비교하여 업데이트

<br/><br/>

# 2장 JSX

JSX는 Javascript의 확장 문법

 중요함 === JSX ≠ Javascript

하지만 JSX는 Javascript의 확장 문법이기 때문에 Javascript 문법 사용 가능

```jsx
//JSX Code
function App() {
  return (
    <div>
      Hello <b>react</b>
    </div>
  );
}
```

JSX 코드는 브라우저에서 실행되기 전에 코드가 번들링 되는 과정에서 바벨을 통해 자바스크립트 형태로 변화하게 되는데 실제로 해당 JSX 코드는

```jsx
function App() {
  return React.createElement(
    'div',
    null,
    'Hello',
    React.createElement('b', null, 'react')
  );
}
```

이러한 형태로 자바스크립트로 변환

만약 React를 통해 컴포넌트들을 렌더링할 때 JSX 코드로 작성하지 않는다면

- 우리는 간단한 코드를 React.createElement..... 이런식으로 작성을 해야함 (매우 귀찮고 불편)

- 고로 우리는 JSX 코드를 사용 편리하게 사용

- 또한 JSX는 마치 HTML 코드를 작성하는 것 처럼 가독성이 매우 뛰어나고 처음 보는 사람도 조금만 보다보면 쉽게 작성할 수 있는 장점이 있고 가장 큰 강점은 활용도

- 우리가 사용하던 HTML 태그들도 JSX 코드 내에서 그대로 사용이 가능하고 아래의 코드처럼 컴포넌트 들도 JSX 코드 안에서 작성이 가능

- 아래와 같이 App 컴포넌트를 HTML 태그처럼 사용 가능

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

JSX는 우리에게 편리함을 제공하지만 그 대가로 우리는 JSX 문법의 몇가지 규칙들을 지켜야함

1. 여러 요소가 있을 경우 부모 태그로 감싸주기
2. Javascript 표현은 {} 로 감싸주기
3. if대신 조건부 연산자(삼항 연산자) 사용하기
4. undefined를 렌더링하지 않기
5. 인라인 스타일링
6. class대신 className을
7. 태그는 반드시 닫아주기
8. 주석은 {/* 이렇게 */}

간단하게 이렇게 8개 정도의 규칙을 지켜준다면 우리는 아주아주 편리한 JSX를 사용함

<br/>

### 1. 여러 요소가 있을 경우 부모 태그로 감싸주기

react의 virtual DOM을 잘 활용하기 위해서 JSX는 여러 요소가 있을 때 부모 태그로 꼭 감싸주어야 에러가 발생하지 않음

```jsx
import React from 'react';

function App(){
	return(
		<h1>리액트 안녕!</h1>
		<h2>잘 작동하니?</h2>
	)
}

export default App;
```

위와 같은 코드를 작성한다면 여러개의 요소가 있을 때 하나의 부모 요소로 감싸져 있지 않으면 아래와 같은 에러가 발생

```jsx
Parsing error: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>…</>?
```

해당코드를 이처럼 div태그로 감싸준다면 더이상 에러가 발생하지 않음

```jsx
import React from 'react';

function App() {
  return (
    <div>
      <h1>리액트 안녕!</h1>
      <h2>잘 작동하니?</h2>
    </div>
  );
}

export default App;
```

하지만 때때로 div 태그를 사용하기를 원하지 않을 수도 있음

따라서 그런 문제를 해결하기 위해 React v16 부터는 div 태그를 대신하여 Fragment 기능을 사용

```jsx
import React, { Fragment } from 'react';

function App() {
  return (
    <Fragment>
      <h1>리액트 안녕!</h1>
      <h2>잘 작동하니?</h2>
    </Fragment>
  );
}

export default App;
```

```jsx
import React from 'react';

function App() {
  return (
    <>
      <h1>리액트 안녕!</h1>
      <h2>잘 작동하니?</h2>
    </>
  );
}

export default App;
```

하지만 Fragment 조차 사용하기 싫면 오른쪽과 같이 <></> 이런 태그로 감싸주게 되면 잘 동작

<br/>

### 2. Javascript 표현식은 {} 로 감싸주기

javascript 표현식을 사용하기 위해서는 {} 로 감싸주면 사용이 가능

아래 코드처럼 name이라는 변수를 {}로 감싸주면 우리가 JSX 코드에서 동적으로 사용

```jsx
import React from 'react';

function App() {
  let name = '리액트';
  return (
    <>
      <h1>{name} 안녕!</h1>
      <h2>잘 작동하니? </h2>
    </>
  );
}

export default App;
```

<br/>

### 3. if대신 조건부 연산자(삼항 연산자) 사용하기

JSX 코드를 사용해서 특정 조건일 때 요소를 화면에 표시할 수 있고 나타나지 않게 할 수 있는데

이건 바로 if 문 대신 조건부 연산자를 사용

```jsx
import React from ‘react‘;

function App() {
  const name = ‘리액트‘;
  return (
    <div>
      {name === ‘리액트‘ ? (
        <h1>리액트입니다.</h1>
      ) : (
        <h2>리액트가 아닙니다.</h2>
      )}
    </div>
  );
}

export default App;
```

```jsx
// 조건이 참 or 거짓일때 각각 표시하고 싶은 내용이 다를때
{조건 ? () : ()}

// 조건이 참일때만 표시하고 싶을때
{조건 ? () : null)}
{조건 && ()}
```

<br/>

### 4. undefined를 렌더링 하지 않기

왠만하면 undefined를 렌더링 하지 않는게 가장 좋지만

특정 데이터를 undefined로 사용해야 한다면 || 연산자를 사용하면 undefined 데이터로 인한 오류를 방지함

<br/>

### 5. 인라인 스타일링

- 리액트는 DOM 요소에 스타일을 적용할 때는 문자열 형태로 넣는 것이 아니라 객체 형태로 넣어주어야 함
- 그렇기 때문에 CSS에서 사용하던 표기법을 그대로 사용하는 것이 아니라 아래의 코드박스처럼

- (하이픈) 표시를 제거하고 카멜 표기법을 이용하여 작성

```jsx
// 기존의 CSS Code
background-color : "blue"

// 리액트에서의 Code ( - 을 뺀 카멜 표기법 사용 )
backgroundColor : "blue"

// ex ..
fontSize : "15px"
fontWeight: "bold"
...
```

하지만 CSS 폴더를 따로 관리하고 사용한다면 기존의 CSS 표기법과 동일하게 사용이 가능

<br/>

### 6. class 대신 className 을 사용하기

HTML에서 CSS를 사용할때 아래와 같이 사용을 하게 되는데 리액트 역시 동일한 방식으로 사용이 가능

다만! HTML에서 사용 했던것 처럼 태그안에 class="class이름" 이렇게 표기하는 것이 아닌 className으로 표기

```jsx
// App.css
.react {
	background: aqua;
	color: black;
	font-size: 48px;
	font-weight: bold;
  padding: 16px;
 }
```

```jsx
import React from 'react';
import './App.css';

function App() {
  const name = '리액트';
  return <div className="react">{name}</div>;
}

export default App;
```

<br/>

### 7. 태그는 반드시 닫아주기

- HTML를 사용할 때 특정 태그들은 태그를 닫지 않고 사용이 가능했음
  - ex) `<br>, <hr>, <input> ...`
- 하지만 React에서는 태그를 닫지 않을 경우 에러가 발생하니 주의
  - ex) `<br></br> or <br/>`

<br/>

### 8. React의 주석은?

- React에서는 {/\* \*/} 이런식으로 작성해 주시면 됩니다.

<br/><br/>

# 3장 Component 와 props 그리고 state

### 컴포넌트란?

- 컴포넌트란 리액트에서 앱을 이루는 최소한의 단위
- 마치 블록처럼 여기기서 가져다 사용할 수 있음

  ( 재사용성이 뛰어나다 , 유지보수도 Good! )

<br/>

### 클래스형 컴포넌트와 함수형 컴포넌트

- 우리가 지금까지 공부하면서 계속 봐왔던 아래와 같은 컴포넌트들은 모두 함수형 컴포넌트
- 함수형 컴포넌트는 function 으로 선언을 하고 사용

<br/>

> 함수형 컴포넌트

- function 이름() 으로 선언

```jsx
import React from 'react';

function App() {
  let name = '리액트';
  return (
    <>
      <h1>{name} 안녕!</h1>
      <h2>잘 작동하니? </h2>
    </>
  );
}

export default App;
```

> 클래스형 컴포넌트

- class 이름 extends Component 으로 선언

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    const name = 'react';
    return <div className="react">{name}</div>;
  }
}
```

### 클래스형 컴포넌트와 함수형 컴포넌트의 차이

- 클래스형 컴포넌트와 함수형 컴포넌트의 가장 큰 차이는 라이프사이클 기능과 state 기능입니다.
- React의 꽃이라고 볼 수 있는 라이프사이클 기능과 state 기능은 함수형 컴포넌트에서 사용하지 못했었습니다.
- 하지만 리액트 v16.8 업데이트 이후 Hooks 기능이 도입되면서 함수형 컴포넌트에서도 라이프사이클 기능, state 기능을 모두 사용할 수 있게 되어 이제는 큰 차이가 없음

> React 개발 팀에서는 앞으로의 React 개발은 함수형 컴포넌트로 개발하는 것을 권장한다고 합니다. 하지만 클래스형 컴포넌트로 개발을 해도 무방합니다.

<br/>

### props 와 state

> props는 용돈! state는 지갑!

props와 state는 각각 속성과 컴포넌트 내부에서 바뀔수 있는 값을 말합니다.<br/><br/>

### props

- props는 컴포넌트 속성을 설정할 때 사용하는 요소
- 단! 부모 컴포넌트에서만 설정할 수 있는 값
- 따라서 읽기 전용으로만 사용이 가능

```jsx
// 부모 컴포넌트 App
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent name="React" />;
};

export default App;
```

```jsx
// 자식 컴포넌트 MyComponent
import React from 'react';

const MyComponent = (props) => {
  return <div>안녕하세요, 제 이름은 {props.name}입니다.</div>;
};

export default MyComponent;
```

<br/>

### state

- state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미
- 컴포넌트 내부에서 변경할 수 있는 값을 지정하여 사용하거나
- 부모 컴포넌트에서 받은 props 값을 state를 통해 변경하여 사용할 수도 있음

```jsx
import React, { Component } from 'react';

class Test extends Component {
	//생성자 props or state를 사용하기 위해서는 꼭 선언해야함
	constructor(props) {
		super(props);
		this.state = {
			money : 0
		};
	}
	render() {
		const { money } = this.state;
		return (
			// 방법 1.
			<h1> 나의 통장 잔고는 {this.state.money} 원.. </h1>
			// 방법 2.
			<h1> 나의 통장 잔고는 {money}원.. </h1>
			<button onClick={() => { this.setState({money: money + 1 });}} />
		);
	}
}
```

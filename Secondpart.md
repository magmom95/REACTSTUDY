![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=300&section=header&text=리액트를%20다루는기술%20&fontSize=90&animation=fadeIn&fontAlignY=38&desc=%20이성규&descAlignY=65&descAlign=90)

* [x] 4장 : 이벤트 핸들링
* [x] 5장 : ref:DOM에 이름 달기
* [x] 6장 : 컴포넌트 반복

## 4장: 이벤트 핸들링

### `HTML` vs `React` (`onClick`)

- HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Title</title>
  </head>
  <body>
    <button onclick="alert('알림입니다.')">Click</button>
  </body>
</html>
```

기존의 `HTML` 에서 `DOM` 요소에 접근을 할 때는 위의 코드와 같이 `" "` 사이에 자바스크립트 코드를 넣거나  
혹은 같은 맥락의 `addEventListener` 를 통해 이벤트 핸들링을 함
`리액트` 에서의 이벤트핸들링은 다음과 같은 차이점을 볼 수 있다.

- REACT
```javascript
import React, { useState } from "react";

const Greeting = () => {
  const [message, setMessage] = useState("");
  const onClickHi = () => {
    setMessage("안녕하세요");
  };
  const onClickBye = () => {
    setMessage("수고하세요");
  };
  return (
    <div>
      <button onClick={onClickHi}>Hi</button>
      <button onClick={onClickBye}>Bye</button>
      <h1>{message}</h1>
    </div>
  );
};

export default Greeting;
```

1. 이벤트 이름은 카멜 표기법으로 작성
 `ex) onclick => onClick`  
 
2. 이벤트에 JS 코드가 아닌, 함수값 형태를 전달 
 `화살표 함수 문법으로 함수를 만들어 전달 or 렌더링 부분 외부에서 미리 만들어 전달 ` 
 
3. 돔(DOM) 요소에만 이벤트를 설정할 수 있다.  
 `만약 돔형식으로 설정하지 않을 경우, 이름이 onClick인 props를 MyComponent에게 전달하게 됨!`
 ```javascript
 ex) <ExComponent onClick={doSomething} />
 ``` 

---

### 간단한 `onChange` 이벤트 예제

```javascript
import React from "react";

const WriteMessage = () => {
  const handleChange = (event) => {
    console.log(event.target.value);
  };
  return (
    <input
      type="text"
      name="message"
      placeholder="Write Something ... 📝"
      onChange={handleChange}
    />
  );
};

export default WriteMessage;
```

<img src="https://user-images.githubusercontent.com/89551626/133533315-264d030a-7218-4b2b-af61-809a2aa64af0.gif">

이런 식으로 `input` 값의 `value` 를 콘솔에 찍어볼 수 있다.  
`onChange` 의 파라미터인 `event` 객체는 `리액트` 의 `Systhetic Event` 로 우리가 기존에 알고 있던 네이티브 이벤트의 객체로 이루어져 있어 비슷하게 사용하면 됨.  
하지만 `Systhetic Event` 가 네이티브 이벤트와 다른 점은 `Event Polling` 인데요, 이벤트가 끝나고 나면 이벤트가 초기화되므로 정보를 참조할 수 없음  

---

### `state` 에 `input` 값 담기

위의 코드에 `state` 를 이용하여 `value` 를 업데이트 시키고 이를 화면에 구현 해봄  
`state 값` 을 지난주에 배운 `세터함수` 를 활용하여 변경시킬 수 있음 
`세터함수` 에 위에서 콘솔에 찍어봤던 `event.target.value` 를 넣고
이후 `input` 의 `value` 에 `state 값` 을 전달해주고 아무 태그나 이용하여 `state 값` 을 찍음

```javascript
import React, { useState } from "react";

const WriteMessage = () => {
  const [message, setMessage] = useState("");
  const handleChange = (event) => {
    setMessage(event.target.value);
  };
  return (
    <>
      <input
        type="text"
        name="message"
        placeholder="Write Something ... 📝"
        value={message}
        onChange={handleChange}
      />
      <h1>내가 쓴 메시지: {message}</h1>
    </>
  );
};

export default WriteMessage;
```

<img src="https://user-images.githubusercontent.com/89551626/133533498-c0775dda-6891-47c9-aa9d-fa7475f098e2.gif">

---
### 리셋 시키기

버튼을 누르면 `input` 의 `value` 을 시키기 위해서 
`세터함수` 를 이용하여 빈 문자열을 넣어주면 됨  

```javascript
// 콜백함수 handleReset
const handleReset = () => {
  setMessage("");
};

// 버튼을 클릭하면 handleReset 실행
<button onClick={handleReset}>초기화</button>;
```

---

### 여러 개의 `input` 다루기

 `input` 이 많을 경우

```javascript
const [writer, setWriter] = useState("");
const [message, setMessage] = useState("");
```

와 같이 `input` 의 개수만큼 `useState` 를 활용해도 되지만 `input` 의 개수가 많아진다면  
위에서 사용한 `event` 객체를 활용하는 방법이 더 효율적임

`state 값` 을 단일 값이 아닌 객체로 선언해보면 아래와 같이

```javascript
const [inputs, setInputs] = useState({
  writer: "",
  message: "",
});
```

이렇게 되면 inputs 에 `writer` 와 `message` 라는 `key` 를 가진 객체가 생성

`writer` 와 `message` 를 자유롭게 쓰기 위해 `Destructing (비구조화 할당)` 을 사용함

```javascript
const { writer, message } = inputs;
```

이제 컴포넌트 내에서 자유롭게 `writer` 와 `message` 를 사용할 수 있음

메시지 작성자인 `writer` field 를 하나 추가하고 리셋 함수인 `handleReset` 까지 수정

```javascript
// handleReset()
const handleReset = () => {
  setInputs({
    writer: "",
    message: "",
  });
};

// writer input field
<input
  type="text"
  name="writer"
  value={writer}
  onChange={handleChange}
/>;
```

이제 `handleChange` 함수만 수정해주면 되는데요, 선행되어야 하는 자바스크립트 지식임
`event.target` 의 `name` 과 `value` 를 둘 다 사용해야하기에 `destructing` 을 이용하여 간단하게 지정
이제 `세터함수` 를 통해 `state 값` 을 변경해줘야함 여기서 앞의 두 자바스크립트 지식을 써야 하는데  
우리는 객체를 업데이트 시켜야 하기 때문에 `spread operator` 를 사용하여야 하며  
사본을 만들고 난 후 그 사본의 상태를 `세터함수` 를 통해 변경하게 되는 것

```javascript
const handleChange = (event) => {
  const { name, value } = event.target;
  setInputs({
    ...inputs,
    [name]: value,
  });
};
```

`inputs[name] = value` 가 안되는 이유는 리액트의 `불변성` 때문인데
기존 상태를 이런 식으로 직접 수정하게 되면 값이 바뀌어도 리렌더링 X  
또한 추후에 컴포넌트 성능 최적화를 위해서는 이 불변성을 지켜줘야함 

<details>
<summary>전체 코드 (클릭하면 볼 수 있음)</summary>
<div markdown="1">

```javascript
import React, { useState } from "react";

const WriteMessage = () => {
  const [inputs, setInputs] = useState({
    writer: "",
    message: "",
  });

  // Destructing
  const { writer, message } = inputs;

  const handleChange = (event) => {
    const { name, value } = event.target;
    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const handleReset = () => {
    setInputs({
      writer: "",
      message: "",
    });
  };

  return (
    <>
      <input
        type="text"
        name="writer"
        value={writer}
        onChange={handleChange}
      />
      <input
        type="text"
        name="message"
        value={message}
        onChange={handleChange}
      />
      <button onClick={handleReset}>초기화</button>
      <h1>작성자:{writer}</h1>
      <h1>내용: {message}</h1>
    </>
  );
};

export default WriteMessage;
```

</div>
</details>

---

## 5장: ref.DOM 에 이름 달기

### ref 는 언제 사용?

`순수 자바스크립트` 를 사용 할 때 특정 DOM 을 선택해야 하는 상황에서는  
`getElementById`, `querySelector` 와 같은 `DOM Selector 함수` 를 사용해서 `DOM` 을 선택

`리액트` 에서도 가끔씩 `DOM` 을 **직접** 선택해야 하는 상황이 있음

ex)
- 특정 `input` 에 포커스 주기
- 스크롤 박스 조작하기
- `Canvas` 요소에 그림 그리기 등

이럴 때 리액트에서는 `ref` 라는 것을 사용

---

1. 콜백함수를 통한 ref 설정   
　ref를 만드는 가장 기본적인 방법은 콜백함수를 사용
　ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달  
  ```javascript
  <input ref={(ref) => {this.input=ref}} />
  ```
　이 때, 이름은 `this.input` 뿐만 아니라 원하는 것으로 자유롭게 설정할 수 있습니다.


2. createRef를 통한 ref 설정   

ref를 만드는 또 다른 방법은 리액트 함수인 `createRef`를 사용  

```javascript
import React, {Component} from 'react';

class RefSample extends Component {
  input = React.createRef();

  handleFocus = () => {
    this.input.current.focus(); //하고싶은 기능 넣기
  };

  render() {
    return (
      <div>
        <input ref={this.input} />
      </div>
    );
  }
}

export default RefSample;
```
위와 같이 설정하고!   
DOM에 접근하려면 this.input.current 처럼 뒤에 .current 를 넣어주면 됨   


리액트에서는 컴포넌트에도 ref를 쓸 수 있음
이 방법은 주로 컴포넌트 내부에 있는 DOM을 포넌트 외부에서 사용할때 씀  
이를 바탕으로 아래와 같이 나옴  

1. 컴포넌트 파일설정   

```javascript
import React, {Component} from 'react';

class ScrollBox extends Component {

    ScrollToBottom = () => {
        const { scrollHeight, clientHeight } = this.box;
        this.box.scrollTop = scrollHeight - clientHeight;
    }
    
  render() {
    const style = {
      border: '1px solid black',
      height: '300px',
      width: '300px',
      overflow: 'auto',
      position: 'relative',
    };

    const innerStyle = {
      width: '100%',
      height: '650px',
      background: 'linear-gradient(white, black)',
    };
    

    return (
      <div
        style={style}
        ref={(ref) => {
          this.box = ref;
        }}
      >
        <div style={innerStyle} />
      </div>
    );
  }
}

export default ScrollBox;
```

2. App 컴포넌트에서(부모) 스크롤박스 컴포넌트 렌더링   
```javascript
import React, {Component} from 'react';

import ScrollBox from './ScrollBox';

class App extends Component{
  render(){
    return(
      <div>
      <ScrollBox ref={(ref)=> this.scrollBox=ref}/>
      <button onClick={()=> this.scrollBox.ScrollToBottom()}>
        맨 밑으로
      </button>
      </div>
    );
  }
}

export default App;
```

#### 결과화면
<kbd><img src="https://user-images.githubusercontent.com/67777124/135083793-31e95afc-2806-4b26-b333-1a9eb7721a6a.png" height="200"/></kbd>

---

## 6장: 컴포넌트 반복

### 6.1 자바스크립트 배열의 map() 함수

`map()` 은 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환

```javascript
const array = [1, 2, 3, 4];
const map = array.map((x) => x * 2);

console.log(map); //Array [2, 4, 6, 8]
```

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### 6.2 데이터 배열을 컴포넌트 배열로 변환하기

`map()` 을 리액트 컴포넌트 내에서 사용하면 아래와 같음

```javascript
import React from "react";

const MapMethod = () => {
  const names = ["눈사람", "얼음", "눈", "바람"];
  const nameList = names.map((name) => <li>{name}</li>);
  return <ul>{nameList}</ul>;
};

export default MapMethod;
```

#### 결과화면
<img src="https://user-images.githubusercontent.com/89551626/135824637-ca0bb3e7-f6cf-4bb2-8a8e-c07800cab2ac.png">

#### BUT 콘솔창을 켜보면 이런 경고메시지가 뜸
<img src="https://user-images.githubusercontent.com/89551626/135824831-10161de4-ac28-4cc2-8b0a-025f3ddfdbc7.png">

### 6.3 key

리액트에서 `key` 는 컴포넌트 배열을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용
`key` 가 있음으로 인해 `Virtual DOM` 을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 더욱 빠르게 감지할 수 있음  
`key` 값은 `props` 와 같이 설정하면 되며 항상 유일한 값이어야 함  
위의 코드를 수정해보면

```javascript
import React from "react";

const MapMethod = () => {
  const names = ["눈사람", "얼음", "눈", "바람"];
  const nameList = names.map((name, index) => <li key={index}>{name}</li>);
  return <ul>{nameList}</ul>;
};

export default MapMethod;
```

이제 경고메시지가 사라졌음을 확인할 수 있음  
하지만 `index` 값은 유동적인 배열일 때 유일한 값이 아니라 계속해서 변할 수 있기 때문에 효율적이지 못함

### 6.4 응용

위를 응용하여 동적인 배열을 효율적으로 렌더링하는 방법을 알아보겠음

```javascript
import React, { useState } from "react";

const MapMethod = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

  const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
  return <ul>{nameList}</ul>;
};

export default MapMethod;
```

`key` 값을 `index` 대신 `name.id` 값으로 지정해주었음  
이제 새로운 데이터를 추가할 수 있는 기능을 구현하면 아래와 같음

**데이터 추가 기능**

```javascript
import React, { useState } from "react";

const MapMethod = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

  const onChange = (event) => setInputText(event.target.value);
  const onClick = () => {
    const nextNames = names.concat({
      id: nextId,
      text: inputText,
    });
    setNextId(nextId + 1);
    setNames(nextNames);
    setInputText("");
  };

  const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button onClick={onClick}>추가</button>
      <ul>{nameList}</ul>
    </>
  );
};

export default MapMethod;
```

여기서 자바스크립트 메소드 `concat()` 이 사용
`concat()` 은 인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환

```javascript
const array1 = ["가", "나", "다"];
const array2 = ["라", "마", "바"];
const array3 = array1.concat(array2);

console.log(array3); // Array ["가", "나", "다", "라", "마", "바"]
```

`push()` 를 사용하지 않는 이유는 `push()` 는 기존 배열 자체를 변경해주어 리액트의 불변성을 깨기 때문입니다. 하지만 `concat()` 은 새로운 배열을 만들어주기 때문에 추후 컴포넌트 성능 최적화가 가능

이제 새로운 데이터를 삭제할 수 있는 기능을 구현 
불변성을 유지하면서 배열의 특정 항목을 지울 때는 `filter()` 를 사용

```javascript
const numbers = [1, 2, 3, 4, 5, 6];
const biggerThanThree = numbers.filter((number) => number > 3);
const withoutThree = numbers.filter((number) => number !== 3);

console.log(biggerThanThree); // Array [4, 5, 6]
console.log(withoutThree); // Array [1, 2, 4, 5, 6]
```

이렇게 `filter()` 를 사용하면 배열에서 특정 조건을 만족하는 원소들만 쉽게 분류 

**데이터 삭제 기능**

```javascript
import React, { useState } from "react";

const MapMethod = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

  const onChange = (event) => setInputText(event.target.value);
  const onClick = () => {
    const nextNames = names.concat({
      id: nextId,
      text: inputText,
    });
    setNextId(nextId + 1);
    setNames(nextNames);
    setInputText("");
  };
  const onRemove = (id) => {
    const nextNames = names.filter((name) => name.id !== id);
    setNames(nextNames);
  };

  const nameList = names.map((name) => (
    <li key={name.id}>
      {name.text}
      <button onClick={() => onRemove(name.id)}>삭제</button>
    </li>
  ));
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button onClick={onClick}>추가</button>
      <ul>{nameList}</ul>
    </>
  );
};

export default MapMethod;
```

### 6.5 정리

- 컴포넌트 배열을 렌더링할 때에는 key 값 설정에 주의
- key 값은 언제나 유일 (중복된다면 렌더링 과정에서 Error)
- 상태 안에서 배열을 변형할 때에는 직접 접근해 수정하는 것이 아니라 concat, filter 등의 배열 내장 함수를 사용해 새로운 배열을 만든 후 새로운 상태로 설정

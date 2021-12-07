![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=300&section=header&text=리액트를%20다루는기술%20&fontSize=90&animation=fadeIn&fontAlignY=38&desc=%20이성규&descAlignY=65&descAlign=90)

* [x] 7장 : 라이프사이클
* [x] 8장 : Hooks
* [x] 9장 : 컴포넌트 스타일링

## 7장

### 7.1 라이프사이클 메서드의 이해

리액트 컴포넌트는 LifeCycle (생명주기) 가 존재함  
컴포넌트의 수명은 페이지가 렌더링되기 전인 준비 과정에서 시작하여 페이지에서 사라질 때 끝나게 됨  
LifeCycle 메소드는 클래스 컴포넌트에서만 사용가능하며 함수에선 Hooks 를 사용하여 비슷하게 사용가능 

LifeCycle 메소드는 총 9가지 이며 접두사로 나누자면,

- **Will ~** = 어떤 작업을 작동하기 **전**에 실행
- **Did ~** = 어떤 작업을 작동한 **후**에 실행

기능에 따라,

- **mount**: 페이지에 컴포넌트가 나타남
- **update**: 컴포넌트 정보를 업데이트
- **unmount**: 페이지에서 컴포넌트가 사라짐
  로 나뉘게 됨

### 7.2 라이프사이클 메서드 살펴보기

#### 마운트

DOM 이 생성되고 웹 브라우저 상에 나타나는 것을 mount임
이 때 호출하는 메소드는 다음과 같다.

1. **constructor**
   : 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메소드로 컴포넌트가 만들어지면 가장 먼저 실행

   ```javascript
   constructor(props) {
       super(props);
       console.log("constructor");
   }
   ```

2. **getDerivedStateFromProps**
   : props 에 있는 값을 state 에 넣을 때 사용하는 메소드

   ```javascript
   static getDerivedStateFromProps(nextProps, prevState) {
       if(nextProps.value !== prevState.value) {    // 조건에 따라 특정값 동기화
           return {value : nextProps.value};
       }
       return null;  // state 를 변경할 필요가 없다면 null 반환
   }
   ```

3. **render**
   : 준비한 UI 를 렌더링하는 메소드로 내부에서 `this.props` 와 `this.state` 에 접근할 수 있으며 리액트 요소를 반환 
   이벤트 설정이 아닌 곳에서 `setState` 를 사용해서는 안되며 DOM 에 접근해도 안됨  
   DOM 정보를 가져오거나 state 에 변화를 줄 때는 `componentDidMount` 에서 처리해야 함

4. **componentDidMount**
   : 컴포넌트가 브라우저 상에 나타난 후 호출하는 메소드로 내부에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, `setTimeout`, `setInterval`, 네트워크 요청과 같은 비동기 작업을 처리하면 됨

#### 업데이트

⭐컴포넌트는 아래의 총 4가지 경우에 업데이트 함(중요)⭐

1. **props 가 바뀔 때**
2. **state 가 바뀔 때**
3. **부모 컴포넌트가 리렌더링 될 때**
4. this.forceUpdate로 강제로 렌더링을 트리거할 때

컴포넌트가 업데이트 되는 시점에 호출되는 메소드들은 다음과 같음

1. **shouldComponentUpdate**
   : `props` 또는 `state` 을 변경했을 때 리렌더링을 시작할지 여부를 지정하는 메소드로 반드시 `true` 또는 `false` 값을 반환해야 
   따로 생성하지 않으면 기본적으로 `true` 값을 반환하며 `false` 값을 반환할 경우 업데이트 과정은 중지됨  
   `props` 와 `state` 는 `this.props` 와 `this.state` 로 접근하고 새로 설정 될 `props` 또는 `state` 는 `nextProps` 와 `nextState` 로 접근할 수 있음  
   이 메소드는 주로 컴포넌트 최적화에 쓰임

```javascript
shouldComponentUpdate(nextProps, nextState) {
    console.log("shouldComponentUpdate", nextProps, nextState);
    // 숫자의 마지막 자리가 4면 리렌더링하지 않습니다
    return nextState.number % 10 !== 4;
}
```

2. **getSnapshotBeforeUpdate**
   : `render` 에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출  
   `componentDidUpdate` 에서 세번째 파라미터인 `snapshot` 값으로 반환하는 값을 전달받을 수 있음
   주로 변화가 일어나기 직전의 DOM 상태를 가져와야 할 때 사용(ex. 스크롤바 위치 유지).

   ```javascript
   getSnapshotBeforeUpdate(prevProps, prevState) {
       if(prevState.array !== this.state.array) {
           const { scrollTop, scrollHeight } = this.list;
           return { scrollTop, scrollHeight };
       }
   }
   ```

3. **componentDidUpdate**
   : 리렌더링을 완료한 후, 화면에 우리가 원하는 변화가 모두 반영되고 나서 호출되는 메소드 
   업데이트가 끝난 직후이므로 DOM 관련 처리를 해도 상관 없음
   `prevProps` 또는 `prevState` 를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있습니다. 또한 `getSnapshotBeforeUpdate` 에서 반환한 값이 있다면 여기서 `snapshot` 값을 전달받을 수 있음

   ```javascript
   componentDidUpdate(prevProps, prevState, snapshot) {
       console.log("componentDidUpdate", prevProps, prevState);
       if (snapshot) {
           console.log("업데이트 되기 직전 색상: ", snapshot);
       }
   }
   ```

#### 언마운트

컴포넌트를 DOM 에서 제거하는 것을 unmount 라고 함

1. **componentWillUnmount**
   : 컴포넌트가 브라우저 상에서 사라지기 전에 호출하는 메소드.
   `componentDidMount` 에서 등록한 이벤트, 타이머, 직접 생성한 DOM 이 있다면 여기서 제거 작업을 해야함

#### 기타

1. **componentDidCatch**
   : 컴포넌트 렌더링 도중 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI 를 보여줄 수 있게 해줌
   ```javascript
   componentDidCatch(error, info) {
       this.setState({
           error: true
       });
       console.log({ error, info });
   }
   ```

여기서 `error` 는 파라미터에 어던 에러가 발생했는지 알려주며, `info` 파라미터는 어디에 있는 코드에서 오류가 발생했느지에 대한 정보를 줌 

### 7.3 라이프사이클 메서드 사용하기

아래의 컴포넌트는 각 라이프사이클 메소드를 실행할 때 마다 콘솔에 기록하고  
부모 컴포넌트에서 `props` 로 색상을 받아 버튼을 누르면 `state.number` 값을 +1 함

- `getDerivedStateFromProps` 는 부모에게서 받은 `color` 값을 `state` 에 동기화하고 있음
- `getSnapshotBeforeUpdate` 는 DOM에 변화가 일어나기 직전의 색상 속성을 `snapshot` 값으로 반환하여 이를 `componentDidUpdate` 에서 조회할 수 있게 함
- `shouldComponentUpdate` 에서 `state.number` 값의 마지막 자리 수가 4일 떄 리렌더링을 취소하도록 설정함

```javascript
// LifeCycleSample.jsx

import React, { Component } from "react";

class LifeCycleSample extends Component {
  state = {
    number: 0,
    color: null,
  };

  myRef = null; // ref를 설정할 부분

  constructor(props) {
    super(props);
    console.log("호출 - constructor");
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    console.log("호출 - getDerivedStateFromProps");
    if (nextProps.color !== prevState.color) {
      return { color: nextProps.color };
    }
    return null;
  }

  componentDidMount() {
    console.log("호출 - componentDidMount");
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log("호출 - shouldComponentUpdate", nextProps, nextState);
    // 숫자의 마지막 자리가 4면 리렌더링 X
    return nextState.number % 10 !== 4;
  }

  componentWillUnmount() {
    console.log("호출 - componentWillUnmonunt");
  }

  handleClick = () => {
    this.setState({
      number: this.state.number + 1,
    });
  };

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log("호출 - getSnapshotBeforeUpdate");
    if (prevProps.color !== this.props.color) {
      return this.myRef.style.color;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log("호출 - componentDidUpdate", prevProps, prevState);
    if (snapshot) {
      console.log("업데이트되기 직전 색상: ", snapshot);
    }
  }

  render() {
    console.log("호출 - render");

    const style = {
      color: this.props.color,
    };

    return (
      <div>
        <h1 style={style} ref={(ref) => (this.myRef = ref)}>
          {this.state.number}
        </h1>
        <p>color: {this.state.color}</p>
        <button onClick={this.handleClick}>더하기</button>
      </div>
    );
  }
}

export default LifeCycleSample;
```

```javascript
// App.jsx

import React, { Component } from "react";
import LifeCycleSample from "./components/LifeCycleSample";

const getRandomColor = () => {
  return "#" + Math.floor(Math.random() * 16777215).toString(16);
};

class App extends Component {
  state = {
    color: "#000000",
  };

  handleClick = () => {
    this.setState({
      color: getRandomColor(),
    });
  };
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>랜덤 색상</button>
        <LifeCycleSample color={this.state.color} />
      </div>
    );
  }
}

export default App;
```

#### 결과화면

- 처음 렌더링 되었을 때:  
  <img src="https://user-images.githubusercontent.com/89551626/136669220-a8699d9b-8132-443b-9d49-c4ab929958f0.png" alt="first rendered" />

- 랜덤 색상 클릭 되었을 때:  
  <img src="https://user-images.githubusercontent.com/89551626/136669231-bfffdf1a-5332-46e4-b3d8-51c9f8358d26.png" alt="random color clicked" />
  
- +1 클릭 되었을 때:  
  ![image](https://user-images.githubusercontent.com/64140544/145019210-761f2958-42d7-49e6-8a75-7dc603222e2b.png)
    
 
이제는 의도적으로 에러를 발생시켜 에러를 잡아내는 방법은 아래와 같다.
`render()` 에서의 에러는 존재하지 않는 함수를 사용하려고 하거나 존재하지 않는 객체의 값을 조회하려고 할 때 주로 발생 
의도적으로 `render()` 에 에러를 발생시켜봄

```javascript
render() {
  console.log("호출 - render");

  const style = {
    color: this.props.color,
  };

  return (
    <div>
      {/* ↓ 없는 객체값 */}
      {this.props.missing.value}
      <h1 style={style} ref={(ref) => (this.myRef = ref)}>
        {this.state.number}
      </h1>
      <p>color: {this.state.color}</p>
      <button onClick={this.handleClick}>더하기</button>
    </div>
  );
}
```

브라우저를 확인하면 이런 에러창이 뜸 
<img src="https://user-images.githubusercontent.com/89551626/136669376-10e8b64c-d908-4e00-abb2-fa0640cd364a.png" alt="intended error" />
이런 에러창이 뜨는 이유는 현재 우리는 개발 서버를 실행중이기 때문
하지만 사용자라면 그냥 빈화면을 보게 됨
이럴 때는 사용자에게 에러가 났음을 인지시켜줘야 

```javascript
// ErrorBoundary.jsx

import React, { Component } from "react";

class ErrorBoundary extends Component {
  state = {
    error: false,
  };
  componentDidCatch(error, info) {
    this.setState({
      error: true,
    });
    console.log({ error, info });
  }
  render() {
    if (this.state.error) return <div>에러가 발생했습니다</div>;
    return this.props.children;
  }
}

export default ErrorBoundary;
```

```javascript
// App.jsx

render() {
  return (
    <div>
      <button onClick={this.handleClick}>랜덤 색상</button>
      <ErrorBoundary>
        <LifeCycleSample color={this.state.color} />
      </ErrorBoundary>
    </div>
  );
}
```

경고창을 닫게 되면 우리가 처리한 대로 에러 발생 문구가 보이게 됨 (유저한테 보여지는 화면)  
<img src="https://user-images.githubusercontent.com/89551626/136669584-ef9ec3fb-4f90-4a48-8ed4-14a5acf9c6ce.png" alt="after error boundary" />

---

## 8장

### 8.1 useState

`useState` 는 가장 기본적인 훅으로 함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해줌  
클래스형 컴포넌트의 `state` 와 `setState()` 와 비슷하다고 생각하면 됨

### 8.2 useEffect

`useEffect` 는 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 훅 
클래스형에서의 `componentDidMount` 와 `componentDidUpdate` 를 합친 형태  
2주차 4장에서의 예시 코드를 그대로 가져와 `useEffect` 를 사용

```javascript
import React, { useEffect, useState } from "react";

const WriteMessage = () => {
  const [inputs, setInputs] = useState({
    writer: "",
    message: "",
  });

  const { writer, message } = inputs;

  // ➕ useEffect 추가 ➕
  useEffect(() => {
    console.log("렌더링 완료");
    console.log({
      writer,
      message,
    });
  });

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

이렇게 빌드를 하면 컴포넌트가 업데이트 될 때마다 렌더링이 된다는 것을 알 수 있음
하지만 컴포넌트가 마운트 될 때만 실행하려면
두번째 파라미터로 빈 배열을 넣어주면 됩니다.

```javascript
useEffect(() => {
  console.log("마운트 될 때만 실행");
}, []);
```

특정 값이 업데이트 될 때만 실행하고 싶다면 두번째 파라미터로 전달되는 배열 안에 검사하고 싶은 요소 값을 넣어주면 됨

```javascript
useEffect(() => {
  console.log(message);
}, [message]);
```

이렇게 두번째 파라미터 배열에 어떤 것을 넣어주냐에 따라 실행되는 조건이 달라집니다.  
만약 컴포넌트가 언마운트 되기 전 이나 업데이트 되기 직전에 어던 작업을 수행하고 싶다면  
`useEffect` 의 `뒷정리(cleanup) 함수` 를 리턴해줘야 합니다.  
코드를 수정해보겠습니다.

```javascript
// WriteMessage.jsx 의 useEffect
useEffect(() => {
  console.log("effect");
  console.log(message);
  return () => {
    console.log("cleanup");
    console.log(message);
  };
}, [message]);
```

```javascript
// App.jsx
import React, { useState } from "react";
import WriteMessage from "./components/WriteMessage";

const App = () => {
  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button
        onClick={() => {
          setVisible(!visible);
        }}
      >
        {visible ? "숨기기" : "보이기"}
      </button>
      <hr />
      {visible && <WriteMessage />}
    </div>
  );
};

export default App;
```

렌더링될 때마다 `cleanup 함수` 가 계속 나타나고 `cleanup 함수` 가 호출될 때는 업데이트 직전 값을 보여줌  
언마운트 될 때만 cleanup 함수를 호출하고 싶다면 `useEffect` 의 두번째 파라미터에 빈 배열을 넣으면 됨

```javascript
useEffect(() => {
  console.log("effect");
  console.log(message);
  return () => {
    console.log("unmount");
  };
}, []);
```

### 8.3 useReducer

`useReducer` 는 `useState` 보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해주고 싶을 때 사용합니다.  
리듀서(reducer) 는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달받아 새로운 상태를 반환하는 함수입니다. 리듀서 함수에서 새로운 상태를 만들 땐느 반드시 불변성을 지켜줘야 함  


```javascript
import React, { useReducer } from "react";

function reducer(state, action) {
  // action 타입에 따라 다른 작업 수행
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      // 아무것도 해당되지 않을 때 기존 상태 리턴
      return state;
  }
}

const Counter = () => {
  // useReducer 첫번째 파라미터 = 리듀서 함수, 두번째 파라미터 = 해당 리듀서 기본 값
  // 결과로 state 값 (현재 가리키고 있는 상태) 그리고
  // dispatch 함수 (액션을 발생시키는 함수) 받아옴
  const [state, dispatch] = useReducer(reducer, { value: 0 });
  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b>
      </p>
      {/* 이런 형태로 함수안에 파라미터로 액션 값을 넣어주면 리듀서 함수 호출되는 구조 */}
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+ 1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>- 1</button>
    </div>
  );
};

export default Counter;
```

(각각 설명 주석 참고)  
`useReducer` 를 사용했을 때의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 것  
이번에는 `input` 상태를 `useReducer` 를 사용하여 관리

```javascript
import React, { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: "",
    nickname: "",
  });

  const { name, nickname } = state;
  const onChange = (event) => {
    dispatch(event.target);
  };

  return (
    <div>
      <input name="name" value={name} onChange={onChange} />
      <input name="nickname" value={nickname} onChange={onChange} />
      <h1>이름: {name}</h1>
      <h1>닉네임: {nickname}</h1>
    </div>
  );
};

export default Info;
```

`useReducer` 에서의 액션은 그 어떤 값도 사용 가능  
그래서 이번에는 이벤트 객체가 지니고 있는 `event.target` 자체를 액션 값으로 사용
이런식으로 `input` 을 관리하면 아무리 그 갯수가 많아져도 코드를 깔끔하게 유지할 수 있음

### 8.4 useMemo

`useMemo` 를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 최적화할 수 있음 

```javascript
import React, { useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중,,");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (event) => {
    setNumber(event.target.value);
  };
  const onInsert = (event) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <p>평균값: {getAverage(list)}</p>
    </div>
  );
};

export default Average;
```

이 경우에는 숫자를 등록할 때 뿐만 아니라 `input` 내용이 수정될 때도 `getAverage` 함수가 호출되는 것을 확인할 수 있음
내용이 바뀔 때는 다시 계산할 필요가 없는데 이렇게 계속 렌더링이 되는 것은 낭비이므로
`useMemo` 를 사용하면 최적화를 시킬 수 있음  
아래처럼 수정하게 된다면 `list` 배열의 내용이 바뀔 때만 `getAverage` 함수가 호출됨

```javascript
import React, { useMemo, useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중,,");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (event) => {
    setNumber(event.target.value);
  };
  const onInsert = (event) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <p>평균값: {avg}</p>
    </div>
  );
};

export default Average;
```

### 8.5 useCallback

`useCallback` 은 `useMemo` 와 상당히 비슷합니다. 주로 렌더링 성능을 최적화해야 하는 상황에서 사용하며 만들어 놨던 함수를 재사용할 수 있음
위에서 구현한 `Average` 컴포넌트는 리렌더링 될 때마다 새로 만들어진 함수를 사용하게 됨  
컴포넌트의 렌더링이 자주 발생하거나 렌더링 해야할 컴포넌트의 개수가 많아지면 최적화가 필요하게 됨  
`useCallback` 을 사용하여 한 번 수정해봄

```javascript
import React, { useCallback, useMemo, useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중,,");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = useCallback((event) => {
    setNumber(event.target.value);
  }, []); // 컴포넌트가 처음 렌더링 될 때만 함수 생성

  const onInsert = useCallback(
    (event) => {
      const nextList = list.concat(parseInt(number));
      setList(nextList);
      setNumber("");
    },
    [number, list]
  ); // number 혹은 list 가 바뀌었을 때만 함수 생성

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <p>평균값: {avg}</p>
    </div>
  );
};

export default Average;
```

`useCallback` 의 첫번째 파라미터에는 생성하고 싶은 함수를 넣고,  
두번째 파라미터에는 배열을 넣으면 됩니다. 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시해야 함
`onChange` 처럼 비어있는 배열을 넣게 되면 컴포넌트가 렌더링될 때 만들었던 함수를 계속해서 재사용하게 되며  
`onInsert` 처럼 배열안에 `number` 과 `list` 를 넣게 되면 인풋 내용이 바뀌거나 새로운 항목이 추가될 때 새로 만들어진 함수를 사용하게 됨

함수 내부에서 상태 값에 의존해야 할 때는 그 값을 반드시 두번째 파라미터 배열 안에 포함시켜 줘야함  
`onChange` 는 기존 값을 조회하지 않고 바로 셋팅만 하기 때문에 비어있어도 상관없지만  
`onInsert` 는 기존의 `number` 과 `list` 를 조회해서 `nextList` 를 생성하기 때문에 배열에 이 두 요소가 꼭 필요함

### 8.6 useRef

`useRef` 는 2주차 5장 `ref` 챕터에서 다뤘으므로 생략

### 8.7 커스텀 Hooks 만들기

여러 컴포넌트에서 비슷한 기능을 공유할 경우 자신만의 훅을 만들어 로직을 재사용할 수 있음  
`useReducer` 에서 썼던 로직을 `useInputs` 라는 훅으로 따로 분리해봄

```javascript
// useInputs.jsx

import { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

export default function useInputs(initialForm) {
  const [state, dispatch] = useReducer(reducer, initialForm);
  const onChange = (event) => {
    dispatch(event.target);
  };
  return [state, onChange];
}
```

```javascript
// Info.jsx

import useInputs from "./useInputs";

const Info = () => {
  const [state, onChange] = useInputs({
    name: "",
    nickname: "",
  });
  const { name, nickname } = state;
  return (
    <div>
      <input name="name" value={name} onChange={onChange} />
      <input name="nickname" value={nickname} onChange={onChange} />
      <h1>이름: {name}</h1>
      <h1>닉네임: {nickname}</h1>
    </div>
  );
};
export default Info;
```

## **9장**

리액트에서 컴포넌트를 스타일링할 때는 다양한 방식을 사용할 수 있음
어떠한 방식이 있는지 알아보고 자주 사용하는 방식을 하나하나 사용해 보자 이 장에서 알아볼 스타일은 아래와 같음

- 일반 css : 컴포넌트를 스타일링하는 가장 기본적인 방식
- Sass: 자주 사용되는 css 전처리기중 하나로 확장된 css 문법을 사용하여 css코드를 더욱 쉽게 작성할 수 있도록 해줌
- CSS Module: 스타일을 작성할 대 css클래스의 고유한 이름을 자동으로 생성해주는 옵션
- styled-components: 스타일을 자바 스크립트 파일에 내장시키는 방식으로 스타일을 작성함과 동시에 적용된 컴포넌트를 만들 수 있게 해줌

### **9.1 가장 흔한 방식, 일반 CSS**

CSS를 작성할 때 가장 중요한 점은 CSS 클래스를 중복되지 않게 만드는 것

**중복되는 것을 방지하는 방법:**

1. 이름을 지을 때 특별한 규칙을 사용하여 짓기
2. CSS selector를 활용하기

### 이름 짓는 규칙

컴포넌트 이름-클래스 형태로 지어 중복되는 것을 방지할 수 있음
비슷한 방식으로 `BEM 네이밍`이라는 방식도 있는데 
이름을 지을 때 일종의 규칙을 준수하여 해당 클래스가 어디에서 어떤 용도로 사용되는지 
명확하게 작성하는 방식 
ex) .card_title-primary

### CSS Selector

CSS Selector를 사용하면 CSS 클래스가 특정 클래스 내부에 있는 경우에만 스타일을 적용할 수 있음

### **9.2 Sass 사용하기**

`Sass`: CSS 전처리기로 복잡한 작업을 쉽게 할 수 있도록 해주고, 
스타일 코드의 재활용성을 높여 줄 뿐만 아니라 
코드의 가독성을 높여서 유지보수를 더욱 쉽게 해줌
Sass에서는 두 가지 확장자 .scss 와 .sass를 지원
두 개의 주요 차이점은 .sass 확장자는 중괄호({})와 세미콜론(;)을 사용하지 않는것!

> 설치
**$npm add node-sass**
> 

**SassComponent.scss 코드**

```jsx
//변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

//믹스인 만들기(재사용되는 스타일 블록을 함수처럼 사용할 수 있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
}

.SassComponent{
  display: flex;
  .box{ //일반 CSS에서는 .SassComponent .box와 마찬가지
    background: red;
    cursor: pointer;
    transition: all 0.3s ease-in;
    &.red{
      //.red 클래스가 .box와 함께 사용되었을 때
      background: $red;
      @include square(1);
    }
    &.orange{
      background: $orange;
      @include square(2);
    }
    &.yellow{
      background: $yellow;
      @include square(3);
    }
    &.green{
      background: $green;
      @include square(4);
    }
    &.blue{
      background: $blue;
      @include square(5)
    }
    &.indigo{
      background: $indigo;
      @include square(6);
    }
    &.violet{
      background: $violet;
      @include square(7);
    }
    &:hover {
      //.box에 마우스를 올렸을 때
      background: black;
    }
  }
}
```

**SassComponent.js 코드**

```jsx
import React from "react";
import "./SassComponent.scss";

const SassComponent = () => {
  return (
    <div className="SassComponent">
      <div className="box red" />
      <div className="box orange" />
      <div className="box yellow" />
      <div className="box green" />
      <div className="box blue" />
      <div className="box indigo" />
      <div className="box violet" />
    </div>
  );
};

export default SassComponent;
```

<kbd><img src="https://user-images.githubusercontent.com/67777124/136947940-f1841488-b206-4f9b-ab7b-7b264c19a6fc.png"></kbd>

### **9.2.1** utils 함수 분리하기

여러 파일에서 사용될 수 있는 Sass 변수 및 믹스인은 다른 파일로 따로 분리하여 작성한 뒤 필요한 곳에서 쉽게 불러와 사용할 수 있음

**src/styles/utils.scss 코드**

```jsx
//변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

//믹스인 만들기(재사용되는 스타일 블록을 함수처럼 사용할 수 있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
```

```jsx
@import './styles/utils';
.SassComponent  {
	display: flex;
		.box {
			(...)
```

### **9.2.2** sass-loader 설정 커스터마이징하기

이 작업은 Sass를 사용할 때 반드시 해야 하는 것은 아니지만, 해두면 유용
예를 들어 SassComponent에서 utils를 불러올 때 프로젝트에 디렉터리를 많이 만들어서 구조가 깊어졌다면 상위 폴더로 한참 거슬러 올라가야하는 단점이 있음
이 문제점은 웹팩에서 Sass를 처리하는 sass-loader의 설정을 커스터마이징하여 해결할 수 있음
create-react-app으로 만든 프로젝트를 커스터마이징하려면 `npm run eject` 명령어를 통해 세부 설정을 밖으로 꺼내 주어야 함

⚠️ 주의점 create-react-app은 기본적으로 git설정이 되어있으니 커밋해 주어야함

`eject`을 하면 프로젝트 디렉터리에 `config`라는 디렉터리가 생성되었을 것
그 디렉터리 안에 있는 `webpack.config.js`에서 `"sassRegex"` 키워드를 찾아 **concat**을 통해 커스터마이징된 sass-loader 설정을 넣어 줌

```jsx
 {
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                    ? shouldUseSourceMap
                    : isEnvDevelopment,
                }.concat({
                  loader: require.resolve("sass-loader"),
                  options: {
                    sassOptions: {
                      includePaths: [paths.appSrc + "/styles"],
                    },
                    sourceMap: isEnvProduction && shouldUseSourceMap,
                  },
                })
              ),
```

`@import 'utils.scss'` 이제부터 utils.scss를 사용하는 컴포넌트가 있다면  
상대경로를 입력할 필요없이 styles 디렉터리 기준 벌대경로를 사용하여 불러올 수 있다.

### **9.2.3** node_modules에서 라이브러리 불러오기

Sass의 장점 중 하나는 라이브러리를 쉽게 불러와서 사용할 수 있다는 점
npm을 통해 설치한 라이브러리를 사용하는 가장 기본적인 방법은 상대 경로를 사용하여 node_modules까지 들어가서 불러오는 방법이 있음
`@import '../../../node_modules/library/styles';`
이보다 더 쉬운 방법은 `@import '~library/styles';`
물결 문자를 사용하면 자동으로 node_modules에서 라이브러리 디렉터리를 탐지하여 스타일을 불러올 수 있음

### **9.3 CSS Module**

CSS Module은 CSS를 불러와 사용할 때 클래스 이름을 고유한 값 형태로 자동으로 만들어서 
컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해주는 기술
css-loader 설정을 할 필요 없이 .
module.css 확장자로 파일을 저장하기만 하면 CSS Module이 적용

**CSS Module.js 코드**

```jsx
import React from "react";
import styles from "./CSSModule.module.css";

const CssModule = () => {
  return (
    <div className={styles.wrapper}>
      안녕하세요 저는 <span className="something">CSS Module!</span>
    </div>
  );
};

export default CssModule;
```

CSS Module이 적용된 스타일 파일을 불러오면 객체를 하나 전달받게 되는데 
CSS Module에서 사용한 클래스 이름과 해당 이름을 고유화한 값이 키-값 형태로 들어 있음
ex) `{ wrapper: "CSSModule_wrapper__1SbdQ" }`

이 고유한 클래스 이름을 사용하려면 클래스를 적용하고 싶은 JSX 엘리먼트에 className={styles.[클래스 이름]} 형태로 전달해 주면 됨

<kbd><img src="https://user-images.githubusercontent.com/67777124/136947500-8c3c16be-3073-47ab-b384-06ac8ba11bb4.png"></kbd>


CSS Module을 사용한 클래스 이름을 두 개 이상 적용할 때는

```jsx
<div className={`${styles.wrapper} ${styles.inverted}`}>
```

템플릿 리터럴 문법을 사용하고 싶지 않다면

```jsx
clssName={[styles.wrapper,styles.inverted].join('')}
```

### **9.4 styled-components**

컴포넌트 스타일링의 또 다른 패러다임은 자바스크립트 파일 안에 스타일을 선언하는 방식
이 방식의 이름은 'CSS-in-JS'
이 중에서 개발자들이 가장 선호하는 `styled-components`대해 알아보자

> 설치$npm install styled-components
> 

장점: `styled-components`를 사용하면 자바스크립트 파일 하나에 스타일까지 작성할 수 있기 때문에 .css 또는 .scss 확장자를 가진 스타일 파일을 따로 만들지 않아도 됨
장점2: props 값으로 전달해 주는 값을 쉽게 스타일에 적용할 수 있음

**StyledComponent.js 코드**

```jsx
import React from "react";
import styled, { css } from "styled-components";

const Box = styled.div`
  /*props로 넣어 준 값을 직접 전달해 줄 수 있습니다*/
  background: ${(props) => props.color || "blue"};
  padding: 1rem;
  display: flex;
`;

const Button = styled.button`
  background: white;
  color: black;
  border-radius: 4px;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  font-size: 1rem;
  font-weight: 600;

  /* &문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
  &:hover {
    background: rgba(255, 255, 255, 0.9);
  }

  /*다음 코드는 inverted 값이 true일 때 특정 스타일을 부여해 줍니다.*/
  ${(props) =>
    props.inverted &&
    css`
      background: none;
      border: 2px solid white;
      color: white;

      &:hover {
        background: white;
        color: black;
      }
    `};
  & + button {
    margin-left: 1rem;
  }
`;

const StyledComponent = () => {
  return (
    <Box color="black">
      <Button>안녕하세요</Button>
      <Button inverted={true}>테두리만</Button>
    </Box>
  );
};

export default StyledComponent;
```
<kbd><img src="https://user-images.githubusercontent.com/67777124/136947551-f8925b18-f621-4246-93a0-c03803d11701.png"></kbd>


### Tagged 템플릿 리터럴

스타일을 작성할 때 ` 을 사용하여 만든 문자열에 스타일 정보를 넣어 줌
여기서 사용한 문법을 `Tagged 템플릿 리터럴`이라고 부름
CSS Module을 배울 때 나온 일반 템플릿 리터럴과 다른 점은 
**템플릿 안에 자바스크립트 객체나 함수를 전달 할 때 온전히 추출할 수 있다는 것**

```jsx
`hello ${{foo: 'bar' }} ${() => 'world'}!`//결과: "hello [object Obect] () => 'world'!"
```

템플릿에 객체를 넣거나 함수를 넣으면 형태를 잃어 버리게 됨
객체는 "[object Object]"로 변환되고, 함수는 함수 내용이 그대로 문자열화 되어 나타남.

함수를 작성하고 나서 해당 함수 뒤에 템플릿 리터럴을 넣어 준다면, 
**템플릿 안에 넣은 값은 온전히 추출 가능.**

```jsx
function tagged(...args){
  console.log(args);
}
tagged`hello ${{foo: 'bar'}} ${ () => 'world'}!`
```

`styled-components`는 이러한 속성을 사용하여 `styled-components`로 만든 컴포넌트의 props를 스타일 쪽에서 쉽게 조회할 수 있도록 해줌
사용해야 할 태그명이 유동적이거나 특정 컴포넌트 자체에 스타일링해 주고 싶다면 다음과 같은 형태로 구현할 수 있음

```jsx
//태그 타임을 styled 함수의 인자로 전달
const MyInput = styled('input')`
  background: gray;
 `

//아예 컴포넌트 형식의 값을 넣어 줌
 const StyledLink = styled(Link)`
   color: blue;
   `
```

### 반응형 디자인

일반 CSS를 사용할 때와 똑같이 media 쿼리를 사용하면 됨
`@media (max-width: 1024px) { width: 768px; }`

`styled-components` 메뉴얼에서 제공하는 유틸 함수 

```jsx
import React from "react";
import styled, { css } from "styled-components";

const sizes = {
  desktop: 1024,
  tablet: 768,
};

//위에 있는 size 객체에 따라 자동으로 media 쿼리 함수를 만들어 줍니다.
const media = Object.keys(sizes).reduce((acc, label) => {
  acc[label] = (...args) => css`
    @medial (max-width: ${sizes[label] / 16}em) {
      ${css(...args)}
    }
  `;
  return acc;
}, {});

const Box = styled.div`
  /*props로 넣어 준 값을 직접 전달해 줄 수 있습니다*/
  background: ${(props) => props.color || "blue"};
  padding: 1rem;
  display: flex;
  /* 기본적으로는 가로 크기 1024px에 가운데 정렬을 하고 가로 크기가 작아짐에 따라
  크기를 줄이고 768px 미만이되면 꽉 채웁니다. */
  width: 1024px;
  margin: 0 auto;
  ${media.desktop`width:768px;`}
  ${media.tablet`width:100%;`};
`;

```

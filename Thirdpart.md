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


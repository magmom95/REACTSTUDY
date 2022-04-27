![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=300&section=header&text=리액트를%20다루는기술%20&fontSize=90&animation=fadeIn&fontAlignY=38&desc=%20이성규&descAlignY=65&descAlign=90)

🎨 Sass 정리 

``` javascript
npm install node-sass
yarn add node-sass
```

- 둘중 하나 입력하면 다운 가능

- scss 문법 사용

1. 변수 사용 

``` javascript
$메인칼라 : #ff0000;

.red {
  color : $메인칼라;
}
```

- $변수명 : 집어넣을값; 이렇게 변수를 만들고 원하는 곳에서 사용가능

2. @import

``` javascript
@import './test.css';
```

``` javascript
body {
  margin : 0;
} 
div {
  box-sizing : border-box;
}
```

- @import 해오면 편리

3. nesting 문법

``` javascript
div.container {
  h4 {
    color : blue;
  }
  p {
    color : green;
  }
}
```

- nesting 문법을 사용

- 셀렉터 해석이 쉽고 class 끼리 관리하기 편해서 사용

4. extends 문법

``` javascript
.my-alert {
  background : #eeeeee;
  padding : 15px;
  border-radius : 5px;
  max-width : 500px;
  width : 100%;
  margin : auto;
}
.my-alert p {
  margin-bottom : 0;
}
```

``` javascript
function Detail(){
  return (
    <div>
      <HTML/>
      <div className="my-alert">
        <p>재고가 얼마 남지 않았습니다</p>
      </div>
    </div>
  )
}
```

- extends를 사용하여 문법 작성

5. @mixin / @include 문법

``` javascript
@mixin 함수() {
  background : #eeeeee;
  padding : 15px;
  border-radius : 5px;
  max-width : 500px;
  width : 100%;
  margin : auto;
}
.my-alert {
  @include 함수()
}

.my-alert p {
  margin-bottom : 0;
}
```

- 보는것 처럼 사용 가능

---

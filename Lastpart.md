![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=300&section=header&text=리액트를%20다루는기술%20&fontSize=90&animation=fadeIn&fontAlignY=38&desc=%20이성규&descAlignY=65&descAlign=90)

* [x] 13장 : 리액트 라우터로 SPA 개발하기
* [x] 14장 : 외부 API를 연동하여 뉴스 뷰어 만들기
* [x] 15장 : Recoil 

## 13장 

### 13.1 SPA란?
[ SPA ]

📌 정의 : `SPA` 는 `Single Page Application` 으로 한 개의 페이지로 이루어진 애플리케이션 전통적인 웹 페이지는 사용자가 이동할 때마다 새로운 `html` 을 받아오고 페이지를 로딩할 때마다 서버에서 리소스를 전달받아 해석한 뒤 화면에 보여 줌 요즘은 웹에서 제공되는 정보가 정말 많기 때문에 새로운 화면을 보여 주어야 할 때마다 서버에서 모든 뷰를 준비한다면 성능상의 문제가 발생할 수 있음

그래서 리액트 같은 라이브러리 혹은 프레임워크를 사용하여 뷰 렌더링을 사용자의 브라우저가 담당하도록 하고 우선 애플리케이션을 브라우저에 불러와서 실행시킨 후에 사용자와의 인터랙션이 발생하면 필요한 부분만 자바스크립트를 사용하여 업데이트 시켜 줌 만약 새로운 데이터가 필요하다면 서버 API 를 호출하여 필요한 데이터만 새로 불러와 애플리케이션에서 사용할 수 있음

싱글 페이지라고 해서 화면이 한 종류만 있는 것은 아님 사용자에게 제공하는 페이지는 한 종류이지만 해당 페이지에서 로딩된 자바스크립트와 현재 사용자 브라우저의 주소 상태에 따라 다양한 화면을 보여줄 수 있음 이렇게 다른 주소에 다른 화면을 보여주는 것을 `Routing` 이라 함 리액트 자체에 내장되어 있지 않아 `react-router` 과 같은 라이브러리를 사용할 수 있음

`SPA` 는 앱의 규모가 커지면 자바스크립트 파일이 커진다는 단점이 있음 페이지 로딩 시 사용자가 실제로 방문하지 않을 페이지의 스크립트 또한 불러오기 때문 추후에 배울 code splitting 으로 라우트별로 파일들을 나누어서 트래픽과 로딩속도를 개선할 수 있음

`react-router` 처럼 브라우저에서 자바스크립트를 사용하여 라우팅을 관리하는 것은 자바스크립트를 실행하지 않는 일반 크롤러에서는 페이지의 정보를 제대로 수집하지 못한다는 단점이 존재 이는 `SSR` 을 통해 해결 가능

📌 기능 : 뷰 렌더링을 사용자의 브라우저가 담당하도록 하고 애플리케이션을 브라우저에 불러와서 실행시킨 후에 사용자와 인터랙션이 발생하면 필요한 부분만 자바스크립트를 사용하여 업데이트, 새로운 데이터가 필요하면 서버 api를 호출하여 필요한 데이터만 새로 불러와 애플리케이션에서 사용할 수 있음

📌 단점 : 앱의 규모가 커지면 자바스크립트 파일이 너무 커짐 ( 페이지 로딩 시 사용자가 실제로 방문하지 않을 수 있는 페이지의 스크립트도 불러옴 → 코드 스플리팅(라우터 별 파일을 나눔)로 트래픽과 로딩 속도 개선

[ 라우팅 ]

- 정의 : 다른 주소에 다른  화면을 보여주는 것
- 리액트  라우팅 라이브러리 :

1. 리액트 라우터  2. 리치 라우터   3. next.js

- 리액트 라우터 : 클라이언트 사이드에서 이뤄지는 라우팅을 간단하게 구현하게 해줌 + 서버 사이드 렌더링때도 라우팅 도와주는 컴포넌트 제공

[ 리액트 라우터 ]

- 사용

  1. 프로젝트 생성 및 라이브러리 설치

     ```jsx
     yarn add react-router-dom
     ```

  2. 프로젝트에 라우터 적용 → src/index.js

     ```jsx
     import { BrowserRouter } from 'react-router-dom';  //추가
     <BrowserRouter>  // 변경
         <App />
       </BrowserRouter>,
     ```

     - BrowserRouter 컴포넌트 기능
       - 페이지를 새로고침 하지않고 주소를 변경, 현재 주소에 관련된 정보를 props로 쉽게 조회하거나 사용할 수 있음
       - 웹 애플리케이션에 HTML5의 History API를 사용하기 때문

  3. 페이지 생성 → 메인 페이지, 불러올 페이지(home.js, About.js)

  4. **Route 컴포넌트** :  특정 주소에 컴포넌트 연결

     - Route 컴포넌트 : 어떤 규칙을 가진 경로에  어떤 컴포넌트를 보여줄지 정의 가능 (사용자의 현재 경로에  따라 다른 컴포넌트를 보여줄 수 있음)
     - 사용법 → app.js

     ```jsx
     <Route  path="주소규칙" component={보여 줄 컴포넌트} />
     ```

     - exact : 중복 렌더링 방지

       ```jsx
       <Route  path="주소규칙" component={보여 줄 컴포넌트} exact={true}/>
       //p.330
       //exact=true는 그 path에서만 보여주고 싶을때,,
       ```

  5. **Link 컴포넌트** : 다른 주소로 이동

     - Link컴포넌트로 페이지 전환 → 페이지를 새로 불러오는게 아니라 애플리케이션은 그대로 유지 + 페이지 주소만 변경 → link컴포넌트 자체는 a 태그로 이뤄졌지만 페이지 전환을 방지하는        기능이 내장되어있음
     - 리액트에서는 일반 웹에서 a태그로 링크달듯 하면 전부다 리렌더링 되기 때문에 사용 x
     - 사용법  → app.js

     ```jsx
     <Link to="주소">내용</Link>
     ```

  6. Route 하나에 여러 path 설정

     - 사용법 : path props를 배열로 설정   → app.js

     ```jsx
     <Route path={['/about', '/info']} component={About} />
     ```
 - 1. URL파라미터 & 쿼리

    2. URL 파라미터 & 쿼리

       페이지 주소를 정의할때 유동적인 값을 전달할때 사용

       1. URL 파라미터  (p.335)

          1. 예시 :  /profile/velopert
          2. 특정 아이디/이름 사용하여 조회할때 사용
          3. 라우트로 사용되는 컴포넌트에서 받아오는 match라는 객체 안의 params 값을 참조. match 객체 안에 현재 컴포넌트가 어떤 경로 규칙에 의해 보이는지 정보가 담김

       2. 쿼리

          1. 예시 :  /about?details=true

          2. 키워드를 검색하거나 페이지에 필요한 옵션을 전달할때 사용

          3. location 객체에 들어있는 search 값에서 조회 가능

             - location 객체는 라우트로 사용된 컴포넌트에게 props로 전달되며, 웹 애플리케이션의 현재 주소에 대한 정보를 지님

             - location 형태

               ```jsx
               <http://localhost:3000/about?detail=true> 주소로 들어갈때
               {
               	"pathname": "/about",
               	"search": "?detail=true",  //쿼리 값 설정
               	"hash": ""
               }
               ```

               - search 값에서 특정 값을 읽어오기 위해 문자열을  qs라는 라이브러리를 사용하여 객체 형태로 변환함

               - 라이브러리 설치 및 import 및 사용

                 1. qs 라이브러리 설치

                 ```jsx
                 yarn add qs
                 ```

                 2. import  및 사용

                 ```jsx
                 import qs from 'qs';
                 
                 const About = ({ location }) => {
                 	const query = qs.parse(location.search, {
                         ifnoreQueryPrefix: true //이 설정을 통해 문자열 맨 앞의 ?를 생략함
                     });
                   const showDetail = query.detail === 'true'; //뭐리의 파싱 결과 값은 문자열입니다.
                 ...
                 ```

  1. 서브 라우트
     - 정의 : 라우트 내부에 또 라우트를 정의하는 것
     - 라우트로 사용되는 컴포넌트 내부에 Route컴포넌트를 또 사용하면 됨

        ```javascript
        // Profiles.jsx

        import React from "react";
        import { Link, Route } from "react-router-dom";
        import Profile from "./Profile";

        const Profiles = () => {
          return (
            <div>
              <h3>사용자 목록</h3>
              <ul>
                <li>
                  <Link to="/profiles/som">Som</Link>
                </li>
                <li>
                  <Link to="/profiles/kev">Kevin</Link>
                </li>
              </ul>

              <Route
                exact
                path="profiles"
                render={() => <div>사용자를 선택해 주세요</div>}
              />
              <Route path="profiles/:username" component={Profile} />
            </div>
          );
        };

        export default Profiles;
        ```

        ```javascript
        // App.jsx
        import React from "react";
        import { Route, Link } from "react-router-dom";
        import About from "./components/react-router/About";
        import Home from "./components/react-router/Home";
        import Profiles from "./components/react-router/Profiles";

        const App = () => {
          return (
            <div>
              <ul>
                <li>
                  <Link to="/">Home</Link>
                </li>
                <li>
                  <Link to="/about">About</Link>
                </li>
                <li>
                  <Link to="/profiles">Profiles</Link>
                </li>
              </ul>
              <hr />
              <Route exact path="/" component={Home} />
              <Route path={["/about", "/info"]} component={About} />
              <Route path="/profiles" component={Profiles} />
            </div>
          );
        };

        export default App;
        ```
        
  2. 리액트 라우터 부가기능
  
  - history  
    `history` 객체는 라우트로 사용된 컴포넌트에 `match`, `location` 과 함께 전달되는 `props` 중 하나로  
    이 객체를 통해 컴포넌트 내에 구현하는 메소드에서 라우터 API 를 호출 함
    예를 들어 특정 버튼을 눌렀을 때 뒤로 가거나, 로그인 후 화면을 전환하거나, 다른 페이지로 이탈하는 것을 방지해야 할 때 이를 활용

  ```javascript
  // HistorySample.jsx
  import React, { Component } from "react";

  class HistorySample extends Component {
    // 뒤로 가기
    handleGoBack = () => {
      this.props.history.goBack();
    };

    // 홈으로 가기
    handleGoHome = () => {
      this.props.history.push("/");
    };

    componentDidMount() {
      // 이것을 설정하고 나면 페이지에 변화가 생기려고 할 때마다 정말 나갈 것인지 질문함
      this.unblock = this.props.history.block("정말 떠날건가?");
    }

    componentWillUnmount() {
      // 컴포넌트가 언마운트되면 질문을 멈춤
      if (this.unblock) {
        this.unblock();
      }
    }

    render() {
      return (
        <div>
          <button onClick={this.handleGoBack}>뒤로</button>
          <button onClick={this.handleGoHome}>홈으로</button>
        </div>
      );
    }
  }

  export default HistorySample;
  ```

  - withRoute  
    `withRouter` 함수는 Higher-order Component 
    라우트로 사용된 컴포넌트가 아니어도 `match`, `location`, `history` 객체를 접근할 수 있게 해줌

  ```javascript
  // WithRouterSample.jsx
  import React from "react";
  import { withRouter } from "react-router";

  const WithRouterSample = ({ location, match, history }) => {
    return (
      <div>
        <h4>location</h4>
        <textarea
          value={JSON.stringify(location, null, 2)}
          rows={7}
          readOnly={true}
        />
        <h4>match</h4>
        <textarea
          value={JSON.stringify(match, null, 2)}
          rows={7}
          readOnly={true}
        />
        <button onClick={() => history.push("/")}>홈으로</button>
      </div>
    );
  };

  export default withRouter(WithRouterSample);
  ```

  - Switch  
     여러 `Route` 를 감싸서 그 중 일치하는 단 하나의 라우트만을 렌더링 시켜 줌  
     `Switch` 를 사용하면 모든 규칙과 일치하지 않을 때 보여줄 Not Found 페이지도 구현
    
  - NavLink  
    `Link` 와 비슷하지만 `Link` 에서 사용하는 경로가 일치하는 경우 특정 스타일 혹은 CSS 클래스를 적용할 수 있는 컴포넌트


### 결론

리액트 라우터를 사용하여 주소 경로에 따라 다양한 페이지를 보여주는 방법
웹 브라우저에 사용할 컴포넌트, 상태 관리를 하는 로직, 그 외 여러 기능을 구현하는 함수들이 점점 쌓이면서
최종 결과물인 자바스크립트 파일의 크기가 매우 커짐

---
       
## 14장

### 14.1 비동기 작업의 이해

서버쪽 데이터가 필요할 때 Ajax기법 사용하여 서버의 API호출하여 데이터 수신
시간이 걸린다! => 작업즉시 처리되는 것이 아닌, 응답을 받을 때까지 기다렸다가 전달받은 응답 데이터를 처리
이 과정은 비동기적으로 처리

비동기적으로 처리한다면, 웹 애플리케이션이 멈추지 않기 때문에 동시에 여러 가지 요청을 처리할 수 있고, 기다리는 과정에서 다른 함수 호출 가능

js에서 비동기작업을 가장 흔히 사용하는 방법 => 콜백함수

파라미터 값이 주어지면 1초 뒤에 10을 더해서 반환하는 함수

```javascript
function increase(number, callback) {
  setTimeout(()=>{
    const result = number + 10;
  if (callback){
    callback(result);
  }
  }, 1000)
}

increase(0, result =>{
  console.log(result);
});
```

1초에 걸쳐서 10,20,30,40과 같은 형태로 여러번 순차적으로 처리하고 싶
아래와 같이 콜백함수 여러번 중첩하여 구현

```javascript
function increase(number, callback) {
  setTimeout(()=>{
    const result = number + 10;
  if (callback){
    callback(result);
  }
  }, 1000)
}
console.log("작업 시작");
increase(0, result =>{
  console.log(result);
  increase(0, result =>{
    console.log(result);
    increase(0, result =>{
      console.log(result);
      increase(0, result =>{
        console.log(result);
        increase(0, result =>{
          console.log(result);
        });
      });
    });
  });
});
```

콜백 안에 또 콜백을 넣어서 구현 하는데, 코드가 복잡해짐
=> 콜백 지옥 발생

**Promise**

Promise는 콜백 지옥 같은 코드가 형성되지 않게 하는 방안

```javascript
function increase(number){
  const promise = new Promise((resolve, reject)=>{
    setTimeout(()=>{
      const result = number + 10;
      if(result>50){
        const e = new Error('NumberTooBig');
        return reject(e);
      }
      resolve(result);
    },1000);
  });
  return promise;
}
increase(0)
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .then(number=>{
    console.log(number);
    return increase(number);
  })
  .catch(e=>{
    console.log(e);
  })
```
작업을 연달아 처리한다고 해서 함수를 여러번 감싸는 것이 아닌, .then을 사용하여 그다음 작업을 설정
=> 콜백지옥 형성x

**async / await**

Promise를 더욱 쉽게 사용할 수 있도록 해주는 ES8 문법 함수 앞부분에 async 키워드 추가하고, 해당 함수 내부에서 Promise의 앞부분에 await키워드를 사용 Promise가 끝날 때까지 기다리고, 결과 값은 특정 변수에 담을 수 있음

### 14.2 axios로 API 호출해서 데이터 받아 오기

axios는 현재 가장 많이 사용되고 있는 자바스크립트 HTTP 클라이언트
HTTP 요청을 Promise 기반으로 처리한다는 점이 특징
```javascript
import React, { useState } from 'react';
import axios from 'axios';

const App = () => {
    const [data, setData] = useState(null);
    const onClick = async () => {
        try {
            const response = await axios.get(
                'https://jsonplaceholder.typicode.com/todos/1',
            );
            setData(response.data);
        } catch (e) {
            console.log(e);
        }
    };
    return (
        <div>
            <div>
                <button onClick={onClick}>불러오기</button>
            </div>
            {data && <textarea rows={7} value={JSON.stringify(data, null, 2)} readOnly={true} />}
        </div>
    )
}
export default App;
```


### 14.3 newsapi API 키 발급받기

<a>https://newsapi.org/s/south-korea-news-api</a> 에 들어가서 회원가입하여 자신의 api키를 받고, `axios.get` 형태로 한국 뉴스 api를 가져 옴

```javascript
import React, { useState } from 'react';
import axios from 'axios';

const App = () => {
  const [data, setData] = useState(null);
  const onClick = async ()=>{
    try{
      const response = await axios.get(
        'https://newsapi.org/v2/top-headlines?counrty=kr&apikey=myapikey',
      );
      setData(response.data);
    } catch(e) {
      consoloe.log(e);
    }
  };
  return (
    <div>
      <div>
        <button onClick={onClick}>불러오기</button>
      </div>
    </div>
  );
};
export defualt App;
```

### 14.4 뉴스 뷰어 UI 만들기

**styled-components**를 이용할 것이므로 `yarn add styled-components`를 이용하여 설치 NewsItem은 뉴스정보를 보여주는 컴포넌트,
```javascript
import React from 'react';
import styled from 'styled-components';

const NewsItemBlock = styled.div`
  display:flex;
  .thumbnail{
    margin-right:1rem;
    img{
      display:block;
      width:160px;
      height:100px;
      object-fit:cover;
    }
  }
  .contents{
    h2{
      margin:0;
      a{
        color:black;
      }
    }
    p{
      margin:0;
      line-height:1.5;
      margin-top:0.5rem;
      white-space:normal;
    }
  }
  &+&{
    margin-top:3rem;
  }
  `;

  const NewsItem = ({article})=>{
    const {title, description, url, urlToImage} = article;
    return(
      <NewsItemBlock>
        {urlToImage && (
          <div className="thumbnail">
            <a href={url} target="_blank" rel="noopener noreferrer">
              <img src={urlToImage} alt="thumbnail"/>
            </a>
          </div>
        )}
        <div className="contents">
          <h2>
            <a href={url} target="_blank" rel="noopener noreferrer">
              {title}
            </a>
          </h2>
          <p>{description}</p>
        </div>
      </NewsItemBlock>
    )
  }
  export default NewsItem;
```
NewsList는 Api를 요청하고 뉴스 데이터가 들어 있는 배열을 컴포넌트 배열로 변환하여 렌더링해 주는 컴포넌트

```javascript
import React from 'react';
import styled from 'styled-components';
import NewsItem from './Newsitem';
import axios from 'axios';
import usePromise from '../lib/usePromise';

const NewsListBlock = styled.div`
  box-sizing: border-box;
  padding-bottom: 3rem;
  width: 768px;
  margin: 0 auto;
  margin-top: 2rem;
  @media screen and(max-width: 768px) {
    width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }
`;
const NewsList = () => {
  return (
    <NewsListBlock>
        <NewsItem article={sampleArticle}/>
        <NewsItem article={sampleArticle}/>
        <NewsItem article={sampleArticle}/>
        <NewsItem article={sampleArticle}/>
        <NewsItem article={sampleArticle}/>
        <NewsItem article={sampleArticle}/>
    </NewsListBlock>
  );
};

export default NewsList;
```

## 14.5 데이터 연동하기
- 컴포넌트가 화면에 보이는 시점에 API요청
useEffect를 사용하여 컴포넌트가 처음 렌더링되는 시점에 API를 요청할 것
useEffect에 등록하는 함수에 async를 붙여서는 안 됨

components/NewsList.js
```Javascript
import React, {useState, useEffect} from 'react';
import styled from 'styled-components';
import NewsItem from './Newsitem';
import axios from 'axios';

const NewsListBlock = styled.div`
  box-sizing: border-box;
  padding-bottom: 3rem;
  width: 768px;
  margin: 0 auto;
  margin-top: 2rem;
  @media screen and(max-width: 768px) {
    width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }
`;
const NewsList = () => {
  const [conticle, setArticles] = useState(null);
  const [loading, setLoading] = useState(null);
  
  useEffect(()=>{
      const fetchData = async () =>{
          setLoading(true);
          try{
              const response = await axios.get(
                  '',
              );
              setArticles(response.data.articles);
          } catch(e){
              console.log(e);
          }
          setLoading(false);
      };
      fetchData();
  },[]);

  if (loading) {
    return <NewsListBlock>대기 중 ...</NewsListBlock>;
  }
  if (!articles) {
    return null;
  }
  if (error) {
    return <NewsListBlock>에러발생!</NewsListBlock>;
  }
  return (
    <NewsListBlock>
      {articles.map((article) => (
        <NewsItem key={article.url} article={article} />
      ))}
    </NewsListBlock>
  );
};

export default NewsList;
```
map함수를 사용하기 전에 꼭 **!articles**를 조회하여 해당 값이 현재 null인지 아닌지 검사
이 작업을 하지 않으려면 아직 데이터가 없을 때 null에는 map함수가 없어 렌더링과정에서 오류 발생


## 14.6 카테고리 기능 구현하기

components/Categories.js

```Javascript
import React from 'react';
import styled from 'styled-components';
import { NavLink } from 'react-router-dom';

const categories = [
  {
    name: 'all',
    text: '전체보기',
  },
  {
    name: 'business',
    text: '비즈니스',
  },
  {
    name: 'entertainment',
    text: '엔터테인먼트',
  },
  {
    name: 'health',
    text: '건강',
  },
  {
    name: 'science',
    text: '과학',
  },
  {
    name: 'sports',
    text: '스포츠',
  },
  {
    name: 'technology',
    text: '기술',
  },
];
const CategoriesBlock = styled.div`
  display: flex;
  padding: 1rem;
  width: 768px;
  margin: 0 auto;
  @media screen and (max-width: 768px) {
    width: 100%;
    overflow-x: auto;
  }
`;

const Category = styled(NavLink)`
  font-size: 1.125rem;
  cursor: pointer;
  white-space: pre;
  text-decoration: none;
  color: inherit;
  padding-bottom: 0.25rem;

  &:hover {
    color: $495057;
  }
  &.active{
      font-weight: 600;
      border-bottom: 2px solid #22b8cf;
      color: #22b8cf;
      &:hover {
        color: #3bc9db;
      }
    }
  &+& {
    margin-left: 1rem;
  }
`;
const Categories = () => {
  return (
    <CategoriesBlock>
      {categories.map((c) => (
        <Category key={c.name}>
          {c.text}
        </Category>
      ))}
    </CategoriesBlock>
  );
};
export default Categories;
```

App.js에 <Categories/>를 호출하여주면, 상단에 카테고리가 나온것을 확인 할 수 있음

## 14.7 리액트 라우터 적용하기

`yarn add react-router-dom`을 이용하여 리액트 라우터를 설치 후,
index.js에 아래 코드를 추가

```Javascript
import {BrowserRouter} from 'react-router-dom';

ReactDom.render()
  <BrowserRouter>
   <App/>
  </BrowserRouter>,
  document.getElementById('root')
);
```

**NewsPage 생성**
```Javascript
import React from "react";
import Categories from "../components/Categories";
import NewsList from "../components/NewsList";

const NewsPage=({match}) =>{
  const category=match.params.category || 'all';

  return(
    <>
    <Categories/>
    <NewsList category={category}/>
    </>
  )
}
export default NewsPage;
```

App.js
```Javascript
import {Route} from 'react-router-dom';
import NewsPage from './pages/NewsPage';

const App = ()=>{
  return <Route Path="/:category?" component={NewsPage}/>;
}
export default App;
```
**Categories에서 NavLink 사용하기**
Categories에서 기존의 onSelect함수를 호출하여 카테고리를 선택하고,
선택한 카테고리에 다른 스타일을 주는 기능을 NavLink로 대체

```Javascript
import React from 'react';
import styled from 'styled-components';
import { NavLink } from 'react-router-dom';

const categories = [
  {
    name: 'all',
    text: '전체보기',
  },
  {
    name: 'business',
    text: '비즈니스',
  },
  {
    name: 'entertainment',
    text: '엔터테인먼트',
  },
  {
    name: 'health',
    text: '건강',
  },
  {
    name: 'science',
    text: '과학',
  },
  {
    name: 'sports',
    text: '스포츠',
  },
  {
    name: 'technology',
    text: '기술',
  },
];
const CategoriesBlock = styled.div`
  display: flex;
  padding: 1rem;
  width: 768px;
  margin: 0 auto;
  @media screen and (max-width: 768px) {
    width: 100%;
    overflow-x: auto;
  }
`;

const Category = styled(NavLink)`
  font-size: 1.125rem;
  cursor: pointer;
  white-space: pre;
  text-decoration: none;
  color: inherit;
  padding-bottom: 0.25rem;

  &:hover {
    color: $495057;
  }
  &.active{
      font-weight: 600;
      border-bottom: 2px solid #22b8cf;
      color: #22b8cf;
      &:hover {
        color: #3bc9db;
      }
    }
  &+& {
    margin-left: 1rem;
  }
`;
const Categories = () => {
  return (
    <CategoriesBlock>
      {categories.map((c) => (
        <Category
          key={c.name}
          activeClassName="active"
          exact={c.name==='all'}
          to={c.name==='all'?'/':`/${c.name}`}
        >
          {c.text}
        </Category>
      ))}
    </CategoriesBlock>
  );
};
export default Categories;

```

## 14.8 usePromise 커스텀 Hook만들기

API호출처럼 Promise를 사용해야 하는 경우 더욱 간결하게 코드를 작성하도록
커스텀 Hook을 만듦

lib/usePromise.js
```Javascript
import {useState, useEffect} from 'react';

export default function usePromise(promiseCreator, deps){
  const [loading, setLoading] = useState(false);
  const [resolved, setResolved] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() =>{
    const process = async()=>{
      setLoading(true);
      try{
        const resolved = await promiseCreator();
        setResolved(resolved);
      }
      catch(e){
        setError(e);
      }
      setLoading(false);
    };
    process();
  // eslint-disable-next-line react-hooks/exhaustive-deps
  },deps);

  return [loading,resolved, error]
}
```
usePromise Hook은 Promise의 완료 결과, 실패 결과에 대한 상태를 관리하며,
usePromise의 의존 배열 deps를 파라미터로 받

components/NewsList.js
```Javascript
import React from 'react';
import styled from 'styled-components';
import NewsItem from './Newsitem';
import axios from 'axios';
import usePromise from '../lib/usePromise';

const NewsListBlock = styled.div`
  box-sizing: border-box;
  padding-bottom: 3rem;
  width: 768px;
  margin: 0 auto;
  margin-top: 2rem;
  @media screen and(max-width: 768px) {
    width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }
`;
const NewsList = ({ category }) => {
  const [loading, response, error] = usePromise(() => {
    const query = category === 'all' ? '' : `&category=${category}`;
    return axios.get(
      `https://newsapi.org/v2/top-headlines?country=kr${query}&apiKey=774f80ed743a469ab6ed074f5c05d334`,
    );
  }, [category]);

  if (loading) {
    return <NewsListBlock>대기 중 ...</NewsListBlock>;
  }
  if (!response) {
    return null;
  }
  if (error) {
    return <NewsListBlock>에러발생!</NewsListBlock>;
  }
  const { articles } = response.data;
  return (
    <NewsListBlock>
      {articles.map((article) => (
        <NewsItem key={article.url} article={article} />
      ))}
    </NewsListBlock>
  );
};

export default NewsList;

```
usePromise를 사용하면 NewsList에서 대기 중 상태 관리와 useEffect설정을 직접 하지 않아도 됨
=> 코드가 간결해짐

상황에 따라 커스텀 Hook을 만들어 적절히 사용

## 14.9 정리
useEffect에 등록하는 함수는 async로 작성하면 안 됨

대신 함수 내부에 async 함수를 따로 만들어주어 사용 

API의 종류가 많아지면 요청을 위한 상태 관리를 하는 것이 번거로워질 수 있음 => 리덕스 미들웨어

---

## recoil

✔ 개념
- atoms에서 selectors(순수 함수)를 거쳐 React컴포넌트로 내려가는 data-flow graph를 만들 수 있음
Atoms는 컴포넌트가 구독할 수 있는 상태의 단위
Selectos는 atoms상태값을 동기 또는 비동기 방식을 통해 변환

### 설치

`npm install recoil` 혹은 `yarn add recoil`

### Recoil 시작하기

### RecoilRoot

```jsx
// App.js

*import* React *from* 'react';
*import* {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
} *from* 'recoil';

*function* App() {
  *return* (
    <RecoilRoot>
      <CharacterCounter />
    </RecoilRoot>
  );
}
```

recoil 상태를 사용하는 컴포넌트는 부모 트리 어딘가에 나타나는 `RecoilRoot`가 필요 루트 컴포넌트가 `RecoilRoot`를 넣기에 가장 좋은 장소

### Atom

Atoms는 **상태(state)의 단위(일부)**이며, 업데이트와 구독이 가능하다. 컴포넌트가 구독할 수 있는 **React state**라고 생각하면 된다. atom이 업데이트되면 각각의 구독된 컴포넌트는 새로운 값을 반영하여 다시 렌더링

**동일한 atom이 여러 컴포넌트에서 사용되는 경우 모든 컴포넌트는 상태를 공유**

- Atom 생성

Atoms는 `atom`함수를 사용해 생성

```jsx
*const* textState = atom({
  key: 'textState',  *// unique ID (다른 atoms/selectors에 대한 고유 ID)*
  *default*: '',  *// default value 기본값 (aka initial value 혹은 초기값)*
});
```

- `CharacterCounter` 컴포넌트 생성

```jsx
function CharacterCounter() {
  return (
    <div>
      <TextInput />
      <CharacterCount />
    </div>
  );
}
```

- `TextInput` 컴포넌트 생성

```jsx
function TextInput() {
  const [text, setText] = useRecoilState(textState); // atom

  const onChange = (event) => {
    setText(event.target.value);
  };

  return (
    <div>
      <input type="text" value={text} onChange={onChange} />
      <br />
      Echo: {text}
    </div>
  );
}
```

컴포넌트에서 atom을 **읽고 쓰려면** `useRecoilState`라는 훅을 사용한다. atom의 값을 구독하여 업데이트할 수 있는 hook이다. React의 `useState`와 비슷하지만 **상태가 컴포넌트 간에 공유될 수 있다**는 차이가 있음 (방식은 동일)

### Selector

Selector는 atoms나 다른 selectors를 입력으로 받아들이는 **순수 함수(pure function)** 상위의 atoms 또는 selectors가 업데이트되면 하위의 selector 함수도 다시 실행 컴포넌트들은 selectors를 atoms처럼 구독할 수 있으며 selectors가 변경되면 컴포넌트들도 다시 렌더링 

Selector는 **파생된 상태(derived state)**의 일부를 나타냄 atom이 상태의 단위이므로, 예를 들어 atom이 데이터 조각이라면 selector는 아톰에서 파생된 데이터 조각이라고 할 수 있음 파생된 상태는 (주어진 상태를 수정하는) 순수 함수에 전달된 상태의 결과물이라고 할 수 있음 파생된 상태는 다른 데이터에 의존하는 동적인 데이터를 만들 수 있음

- Selector 정의

Selectors는 `selector`함수를 사용해 정의

```jsx
const charCountState = selector({
  key: "charCountState",
  get: ({ get }) => {
    const text = get(textState);
    return text.length;
  },
});
```

`get`은 **계산될** 함수 전달되는 `get` 인자를 통해 atoms와 다른 selectors에 접근 가능

✔ 즉, atom인 `textState`를 받아와 `text.length`로 반환하는 파생된 데이터를 만듬

- `CharacterCount` 컴포넌트 생성

```jsx
function CharacterCount() {
  const count = useRecoilValue(charCountState);
  return <> Character Count : {count}</>;
}
```

Selectors는 `useRecoilValue()`를 사용해 **읽을 수 있음**. `useRecoilValue()`는 하나의 atom이나 selector를 인자로 받아 대응하는 값을 반환 **즉, setter 함수 없이 atom의 값을 반환** `**get` 함수만 제공되면 selector는 읽기만 가능한 `RecoilValueReadOnly` 객체를 반환\*\* 따라서 `charCountState` selector는 writable하지 않기 때문에 `useRecoilState()`를 이용X

### 전체 코드

![image](https://user-images.githubusercontent.com/64140544/155710733-f89c1bb0-dc11-457f-855e-0cb0e30dcb63.png)

```jsx
import React from "react";
import {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
} from "recoil";

// RecoilRoot
export default function App() {
  return (
    <RecoilRoot>
      <CharacterCounter />
    </RecoilRoot>
  );
}

// Atom
*const* textState = atom({
  key: 'textState',
**  *default*: '',
**});

// * * * * * * * * * * * * * * * * * * * * * * * * * * * //

function CharacterCounter() {
  return (
    <div>
      <TextInput />
      <CharacterCount />
    </div>
  );
}

// * * * * * * * * * * * * * * * * * * * * * * * * * * * //

function TextInput() {
  const [text, setText] = useRecoilState(textState);
  const onChange = (event) => {
    setText(event.target.value);
  };
  return (
    <div>
      <input type="text" value={text} onChange={onChange} />
      <br />
      Echo: {text}
    </div>
  );
}

// Selector
const charCountState = selector({
  key: "charCountState",
  get: ({ get }) => {
    const text = get(textState);
    return text.length;
  },
});

function CharacterCount() {
  const count = useRecoilValue(charCountState);
  return <> Character Count : {count}</>;
}
```

### todo 리스트 애플리케이션

### 도입부(RecoilRoot)

```jsx
import React, { useState } from "react";
import {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
  useSetRecoilState,
} from "recoil";

export default function App() {
  return (
    <RecoilRoot>
      <TodoList />
    </RecoilRoot>
  );
}
```

- `TodoList`
  - `TodoListStats`
  - `TodoListFilters`
  - `TodoItemCreator`
  - `TodoItem`

### Atoms

- `TodoList`
  ```jsx
  const todoListState = atom({
    key: "todoListState",
    default: [],
  });

  function TodoList() {
    const todoList = useRecoilValue(filteredTodoListState);

    return (
      <>
        <TodoListStats />
        <TodoListFilters />
        <TodoItemCreator />
        {todoList.map((todoItem) => (
          <TodoItem key={todoItem.id} item={todoItem} />
        ))}
      </>
    );
  }
  ```
  - `TodoItemCreator`
  ```jsx
  function TodoItemCreator() {
    const [inputValue, setInputValue] = useState("");
    const setTodoList = useSetRecoilState(todoListState);

    const addItem = () => {
      setTodoList((oldTodoList) => [
        ...oldTodoList,
        {
          id: getID(),
          text: inputValue,
          isComplete: false,
        },
      ]);
      setInputValue("");
    };

    const onChange = ({ target: { value } }) => {
      setInputValue(value);
    };

    return (
      <div>
        <input type="text" value={inputValue} onChange={onChange} />
        <button onClick={addItem}>Add</button>
      </div>
    );
  }

  // 고유 ID
  let id = 0;
  function getID() {
    return id++;
  }
  ```
  - `TodoItem`
  ```jsx
  function TodoItem({ item }) {
    const [todoList, setTodoList] = useRecoilState(todoListState);
    const index = todoList.findIndex((listItem) => listItem === item);

    const editItemIndex = ({ target: { value } }) => {
      const newList = replaceItemAtIndex(todoList, index, {
        ...item,
        text: value,
      });
      setTodoList(newList);
    };

    const toggleItemCompletion = () => {
      const newList = replaceItemAtIndex(todoList, index, {
        ...item,
        isComplete: !item.isComplete,
      });
      setTodoList(newList);
    };

    const deleteItem = () => {
      const newList = removeItemAtIndex(todoList, index);
      setTodoList(newList);
    };

    return (
      <div>
        <input type="text" value={item.text} onChange={editItemIndex} />
        <input
          type="checkbox"
          checked={item.isComplete}
          onChange={toggleItemCompletion}
        />
        <button onClick={deleteItem}>X</button>
      </div>
    );
  }

  function replaceItemAtIndex(arr, index, newValue) {
    return [...arr.slice(0, index), newValue, ...arr.slice(index + 1)];
  }

  function removeItemAtIndex(arr, index) {
    return [...arr.slice(0, index), ...arr.slice(index + 1)];
  }
  ```
---

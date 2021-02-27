# import 구문. 
```
import React from 'react';
```
리액트 프로젝트를 만들 때, node_modules 라는 디렉터리도 함께 생성됨. 프로젝트 생성 과정에서 node_modules 디렉토리에 react 모듈이 설치된다. import 구문을 통해 리액트를 불러와서 사용함.
모듈을 불러와서 사용하는 것은 사실 원래 브라우저에 없는 기능. 브라우저가 아닌 환경에서 자바스크립트를 실행할 수 있게 해 주는 환경인 node.js에서 지원하는 기능. node.js 에서는 require 구문을 쓴다. 
이러한 기능을 브라우저에서도 사용하기 위해 번들러를 사용한다. 파일을 묶어 연결하는 것. 대표적인 번들러로 웹팩, parcel 등이 있다. 각 도구마다 특성이 있음. 주로 웹펙을 사용하는 추세. 번들러 도구를 사용하면 import(require) 로 모듈을 모두 합쳐서 하나의 파일을 생성해줌. 또 최적화 과정에서 여러 개의 파일로 분리될 수도 있음. 
```
import logo from '/.logo.svg';
import '/App.css';
```
웹팩을 사용하면, svg파일과 css 파일도 불러와서 사용할 수 있다. 이렇게 파일들을 불러오는 것은 웹팩의 로더라는 기능이 담당한다. 예를들어, css-loader는 css파일을 불러올 수 있게 해주고, file-loader는 웹 폰트나 미디어 파일들을 불러올 수 있게 해준다. 그리고 babel-loader는 자바스크립트 파일들을 불러오면서 최신 자바스크립트 문법으로 작성된 코드를 바벨이라는 도구를 사용하면 es5 문법으로 변화해줌. 

```
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}
```
App이라는 컴포넌트를 만들어 준다. 이러한 형태를 함수형 컴포넌트라고 부른다. 프로젝트에서 컴포넌트를 렌더링하면, 함수에서 반환하고 있는 내용을 나타낸다. redering은 보여준다는 의미. HTML을 반환하는 것처럼 보이지만, JSX를 반환하는 것. 

# JSX
JSX는 자바스크립트의 확장 문법이며, xml과 매우 비슷하게 생김. 이런 형식으로 작성한 코드는 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다. 

```
function App(){
  return (
    <div>Hello <b>react </b>
    </div>
    );
}
```
가 아래의 코드로 변환된다. 매변 React.createElement 함수를 사용하기에 불편함. 
```
function App(){
    return React.createElement("div",null,"Hello",React.createElement("b",null,"react"));
}
```

# JSX의 장점 
보기 쉽고 익숙하다. 높은 활용도. html 태그 쓰듯이 사용할 수 있다. 

```
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```
# ReactDOM.render 이란? 
컴포넌트를 페이지에 렌더링 하는 역할을 하며, react-dom 모듈을 불러와 사용할 수 있다. 이 함수의 첫 번째 파라미터에는 페이지에 렌더링한 내용을 JSX 형태로 작성하고, 두 번째 파라미터는 해당 JSX를 렌더링 할 document 내부 요소를 설정한다. 여기서 id가 root인 요소 안에 렌더링을 하도록 설정함.  이 요소는 public/index.html 파일을 열어보면 있음. 

# React.StrictMode란?
리액트 프로젝트에서 리액트의 레거시 기능들을 사용하지 못하게 하는 기능이다. 이를 사용하면 문자열 ret, componentWillMount 등 나중에는 완전히 사라지게 될 옛날 기능을 사용했을 때 경고를 출력한다. 

# 여러개의 요소를 하나의 요소로 감싸 주어야 함. 
```
import React from 'react';
import './App.css';

function App() {
  return (
    <div>
      <h1> 리액트 안녕!</h1>
      <h2> 잘 작동하니?</h2>
    </div>
  )
}

export default App;
```
왜냐하면 virtual DOM에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙이 있기 때문임. div 요소를 사용하고 싶지 않으면, 리액트 v16 이상부터 도입 된 Fragment라는 기능을 사용함. 

```
import React, {Fragment} from 'react';
import './App.css';

function App() {
  return (
    <Fragment>
      <h1> 리액트 안녕!</h1>
      <h2> 잘 작동하니?</h2>
    </Fragment>
  );
}

export default App;
```
# ES6의 const 와 let 
```
import React, { Fragment } from 'react';
import './App.css';

function App() {
  const name = 'Mia';
  return (
    <>
      <h1> {name} 안녕!</h1>
      <h2> 잘 작동하니?</h2>
    </>
  );
}
```

export default App;
const는 es6 문법에서 새로 도입되었으며, 한번 지정하고 나면 변경이 불가능한 상수를 선언할 때 사용하는 키워드 입니다. let은 동적인 값을 담을 수 있는 변수를 선언할 때 사용하는 키워드입니다. var 키워드는 scope가 함수 단위이다. let과 const는 scope가 함수 단위가 아닌 블록 단위이므로, if 문 내부에서 선언한 a값은 if 문 밖의 a값을 변경하지 않음. let과 const를 사용할 때 같은 블록 내부애서 중복 선언이 불가능하다 

```
let a = 1;
let a = 2 ; //error
```
그리고 const는 한번 선언하면 재설정 할 수 없습니다.
```
const b =1 ;
b=2 ;  //error
```
즉 let은 한번 선언한 후 값이 유동적으로 변할 수 있을 때만 사용하고, const는 한번 설정한 후 변할 일이 없는 값에 사용합니다. 기본적으로 const를 사용하고, 해당 값을 바꾸어야 할 때는 let을 사용. 

# if문 대신 조건부 연산자. 
JSX 내부의 자바스크립트 표현식에서 if 문을 사용할 수 없다. 하지만 조건에 따라 다른 내용을 렌더링 해야 할 때는 JSX 밖에서 if문을 사용하거나 사전에 값을 설정하거나, {} 안에 조건부 연산자를 사용하면 됩니다. 삼항연산자를 사용하는 것. 

```
import React from 'react';
import './App.css';

function App() {
  const name = 'Mia';
  return (
    <div>
      {name === 'Mia' ? (<h1>Hello Mia:)</h1>) : (<h2> you are not Mia:)</h2>
      )}
    </div>
  );
}

export default App;

```

# AND 연산자(&&)를 사용한 조건부 렌더링
특정 조건을 만족할 때 내용을 보여주고, 만족하지 않을 때 아예 아무것도 렌더링 하지 않아야 하는 상황이 올 수 있다. 이럴 때도 조건부 연산자를 통해 구현할 수 있다. 
```
import React from 'react';
import './App.css';

function App() {
  const name = 'lala';
  return (
    <div>
      {name === 'Mia' ? <h1>Hello Mia:)</h1> : null
      }
    </div>
  );
}

export default App;

```
# && 연산자를 사용해 조건부 렌더링 함. 
리액트에서 false를 렌더링할 때는 null과 마찬가지로 아무것도 나타나지 않기 때문. 여기서 한가지 주의해야 할 점은, falsy한 값인 0은 예외적으로 화면에 나타난다. 
```
import React from 'react';
import './App.css';

function App() {
  const name = 'MIA';
  return <div>{name === 'MIA' && <h1>MIA:) 입니다. </h1>}</div>;
}

export default App;
```
# JSX는 언제 괄호로 감싸나? 
JSX룰 여러 줄로 작성할 때 괄호로 감싸고, 한 줄로 표현할 수 있는 JSX는 감싸지 않는다.

# undefined 렌더링 하지 않기. 
```
import React from 'react';
import './App.css';

function App() {
  const name = undefined;
  return name
}

export default App;

```
에러가 뜬다. ,or연산자를 사용하면 해당값이 undefined 일 때 사용할 값을 지정할 수 있다. 
```
import React from 'react';
import './App.css';

function App(){
  const name = undefined;
  return name || '값이 undefined 입니다';
}
export default App;

or 
//undefined를 내주고 싶을때. 
function App(){
  const name = undefined;
  retunr <div>{name}<div>;
}
export default App;

function App(){
  const name = undefined;
  return <div>{name|| '리액트'}</div>
export default App;

}

```

# 인라인 스타일링
리액트에서 DOM 요소에 스타일을 적용할 때는 문자열 형태로 넣는 것이 아니라 객체 형태로 넣어 주어야 한다. 스타일 이름 중에서 background-color 처럼 문자가 포함되는 이름이 있는 경우, 아러한 이름은 카멜 표기법으로 작성해야 한다.

```
import React from 'react';

function App(){
  const name = '리액트';
  const style = {
    backgroundColor: 'black',
    color: 'pink',
    fontSize:'48px',
    fontWeight:'bold',
    padding:20 //단위를 생략하면 px 가 디폴트 
  }
  return <div style ={style}> {name}</div>;
}

//or 
function App(){
  const name = '리액트';
  return (
    <div 
    style ={{
      backgroundColor: 'black',
      color: 'pink',
      fontSize:'48px',
      fontWeight:'bold',
      padding:20 //단위를 생략하면 px 가 디폴트 
    }}
    >
    {name}
    </div>
  );
}

export default App;
```

# class 대신 className
일반 HTML에소 CSS클래스를 사용할 때 <div class="my_class"> </div>와 같이 class라는 속성을 설정. 하지만 JSX에서는 class가 아닌 className으로 설정해 주어야 한다. 
```
//app.js
function App() {
  const name = '리액트';
  return <div className="react">{name}</div>;
}
export default App;
```
```
//app.css
.react{
  background-color: yellow;
  color: black;
  font-size: 50px;
  font-weight: lighter;
  padding: 20px;
}
```

# 태그는 앞과 뒤가 꼭 닫혀있어야 한다. 
html은 ```<input>```만 써도 가능하지만, JSX는 오류가 된다. 그러므로 ```<input></input>```으로 해야함. 
# JSX 내부에 주석을 달려고 할때, {/* ----- */} 이런식으로 작성해야 한다. 
# 함수형 컴포넌트
```
//함수형 컴포넌트 
function App(){
    const student = 'mia';
    return <div className='student'> {student} <div>
}
export default App;
```
# 클래스형 컴포넌트 
```
import React, {Component} from 'react';
class App extends Component{
    render(){
        const student = 'mia';
        return <div className ='student'> {student}</div>
    }
}

export default App;
```

# 클래스형 컴포넌트와 함수형 컴포넌트의 차이점은? 
클래스 컴포넌트의 경우 state기능 및 라이프사이클 기능을 사용할 수 있다는 점과 임의 메서드를 정의할 수 있다는 점. 클래스형 컴포넌트에서는 reder 함수가 꼭 존재해야 하고, 그 안에서 보여 주어야 할 JSX를 반환하는 것이다. 

함수형 컴포넌트의 장점: 클래스형 컴포넌트보다 선언하기가 편하고, 메모리 지원도 클래스형 컴포넌트보다 덜 사용. 프로젝트를 완성하여 빌드 한 후 배포할 때도 함수형 컴포넌트를 사용하는 것이 결과물의 파일 크기가 더 작다. 그러나 함수형 컴포넌트와 클래스형 컴포넌트는 성능과 파이 크기 면에서 사실상 별 차이가 없음.
함수형 컴포넌트의 단점 : state와 라이프사이클 API의 사용이 불가능하다. 이러한 단점은 Hooks라는 기능이 도입되면서 해결됨. 완전한 클래스형 컴포넌트와 똑같이 사용할 수 있는 것은 아니지만 비슷한 작업을 할 수 있다. 리액트 공식 메뉴얼에서는 컴포넌트를 새로 작성할 때 함수형 컴포넌트와 Hooks를 사용하도록 권장함. 

# 컴포넌트 생성 방법
1. 컴포넌트 파일 만들기
2. 코드 작성하기
3. 모듈 내보내기 및 불러오기 

# ES6  화살표 함수 
 화살표 함수공부 추가. 

 # props
 props는 properties를 줄인 표현, 컴포넌트 속성을 설정할 때 사용하는 요소. props 값은 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 설정할 수 있음. App컴포넌트가 부모 컴포넌트 MyComponent에서
 ```
 import React from 'react';

const MyComponent = props => {
    return <div> Hello, My name is {props.name}</div>
};

export default MyComponent;
```

```
import React from 'react';
import MyComponent from './MyComponent'
import './App.css';

const App = () => {
  return <MyComponent name="MiA" />;// or </MyComponent> 
}

export default App;

```
# default Props
props 값을 따로 지정하지 않을 때 보여 줄 기본값을 설정하는 defaultProps. 
```
import React from 'react';

const MyComponent = props => {
    return <div> Hello, My name is {props.name}</div>
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}

export default MyComponent;
```
```
import React from 'react';
import MyComponent from './MyComponent'
import './App.css';

const App = () => {
  return <MyComponent />;// or </MyComponent> 
}

export default App;

```

# 태그 사이의 내용을 보여주는 children 
리액트 컴포넌트를 사용할 때 컴포넌트 태그 사이의 내용을 보여주는 props는 children이다. 
```
import React from 'react';

const MyComponent = props => {
    return <div> Hello, My name is {props.name} <br />
    children 값은 {props.children} 입니다
    </div >
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}

export default MyComponent;
```

```
import React from 'react';
import MyComponent from './MyComponent'
import './App.css';

const App = () => {
  return <MyComponent>MIA </MyComponent>
}

export default App;

```

# 비구조화 할당 문법을 통한 props 내부 값 추출. 

```
import React from 'react';

const MyComponent = props => {
    const { children, name } = props;
    return <div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
    </div >
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}

export default MyComponent;
```
# 더 간결하게 
```
import React from 'react';

const MyComponent = ({ name, children }) => {
    return <div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
    </div >
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}

export default MyComponent;
```
# propTypes를 통한 props 검증 
컴포넌트의 필수 props를 지정하거나 props 타입을 지정할 때는 propTypes 사용합니다. 
```
import React from 'react';
import PropTypes from 'prop-types'

const MyComponent = ({ name, children }) => {
    return <div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
    </div >
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}
MyComponent.propTypes = {
    name: PropTypes.string
};

export default MyComponent;
/* Mycomponent.propTypes 의 p는 소문자 이건 Mycomponent안에 있는 메소드니까 propTypes*/
```

# isRequired를 사용히야 필수 propTypes 설정. 
propTypes를 지정하지 않았을 때 경고 메시지를 띄워 주는 방법. propTypes를 지정할때 isRequired를 붙여 주면 된다. 
```
import React from 'react';
import PropTypes from 'prop-types'

const MyComponent = ({ name, favoritNumber, children }) => {
    return (<div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
        <br />
    My favorite number is {favoritNumber}.
    </div >
    );
};
MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}
MyComponent.propTypes = {
    name: PropTypes.string,
    favoritNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

# 클래스형 컴포넌트에서 props 사용하기 
클래스형 컴포넌트에서 props를 사용할 때는 render 함수에서 this.props를 조회하면 된다. 그리고 defaultProps와 propTypes는 똑같은 방법으로 설정. 
```
import React, { Component } from 'react';
import PropTypes from 'prop-types'

class MyComponent extends Component {
    render() {
        const { name, favoritNumber, children } = this.props;
        return (
            <div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
                <br />
    My favorite number is {favoritNumber}.
            </div >

        )
    }
}

MyComponent.defaultProps = {
    name: '기본이름 : Mia '
}
MyComponent.propTypes = {
    name: PropTypes.string,
    favoritNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

```
class MyComponent extends Component {
    static defaultProps = {
        name: "default name"
    };

    static propTypes = {
        name: PropTypes.string,
        favoritNumber: PropTypes.number.isRequired
    };

    render() {
        const { name, favoritNumber, children } = this.props;
        return (
            <div> Hello, My name is {name} <br />
    children 값은 {children} 입니다
                <br />
    My favorite number is {favoritNumber}.
            </div >
        )
    }
}
```

# 더 많은 PropTypes 종류 
array: 배열
arrayOf(다른 propType): 특정 PropTypes으로 이루어진 배열을 의미. 
bool : true or false
func : 함수
number : 숫자
object : 객체
string = 문자열
symbol = es6 symbol 
node = 렌더링 할 수 있는 모든 것 (숫자, 문자열, JSX 코드, children 모듀 node PropType)
instanceOf(클래스 ) = 특정 클래스의 인스턴스 (에 : instanceOf(MyClass))
oneOf(['dog', 'cat']) = 주어진 배열 요소 중 값 하나 
oneOfType([React.PropTypes.sturing, PropTypes.number]) = 주어진 배열 안의 종류 중 하나 
objectOf(React.PropTypes.number) = 객체의 모든 키 값이 인자로 주어진 ProType 객체 
shape({name :PropTypes.string, num:PropTypes.number}) = 주어진 스키마를 가진 객체 
any : 아무 종류 

# state
state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미. props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이며, 컴포넌트 자신은 해당 props를 읽기 전용으로만 사용할 수 있다. props를 바꾸려면 부모 컴포넌트에서 바꾸어주어야 한다. 예를 들어, App컴포넌트에서 MyComponent를 사용할 때 props를 바꾸어 주어야 값이 변경ㅇ 될 수 있다. 반면 MyComponent에서 전달받은 name 값을 직접 바꿀 수 없음.
```
import React, { Component } from 'react';

class Counter extends Component {
    constructor(props) {
        super(props);
        this.state = {
            number: 0
        };
    }
    render() {
        const { number } = this.state;
        return (
            <div>
                <h1>{number}</h1>
                <button
                    onClick={() => {
                        this.setState({ number: number + 1 });
                    }}>
                    +1
                </button>
            </div>
        );
    }
}

export default Counter;
```
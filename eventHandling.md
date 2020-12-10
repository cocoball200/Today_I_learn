# 이벤트를 사용할 때 주의사항
1. 이벤트 이름은 카멜 표기법으로 작성한다. 
HTML의 onclick은 리액트에서 onClick으로 작성되어야함. 
2. 이벤트를 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달한다.
HTML에서 이벤트를 설정할 때는 큰 따옴표 안에 실행할 코드를 넣지만, 리액트에서는 함수 형태의 객체를 전달. 
3. DOM 요소에만 이벤트를 설정할 수 있다. 
div,button,input,form,span 등의 DOM 요소에는 이벤트를 설정할 수 있지만, 우리가 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없다. 
예를들어, MyComponent에 onClick 값을 설정한다면 MyComponent를 클릭할 때 dosomething 함수를 실행하는 것이 아니라, 그냥 이름이 onClick인 props를 MyComponent에게 전달 해줌 .
```
<MyComponent onClick = {dosomething}/>
```
따라서 컴포넌트 자체적으로 이벤트를 설정할 수 없다. 하지만 전달받은 props를 컴포넌트 내부의 DOM에 이벤트를 설정할 수 있다. 
```
<div onClick = {this.props.onClick}>
{....}
</div>
```
# 이벤트의 종료 
https://reactjs.org/docs/events.html

# 이벤트 핸들링 동작 방법
1. 컴포넌트 생성 및 불러오기
2. onChange 이벤트 핸들링하기
3. 임의 메서드 만들기
4. input 여러개 만들기
5. onKeyPress 이벤트 핸들링 만들기 . 


# 1. 컴포넌트 생성 및 불러오기 

# e.target.value 
이벤트의 내부 값을 담는 것
``` 
import React, { Component } from 'react';

class EventPractice extends Component {
    render() {
        return (
            <div>
                <h1>이벤트 연습</h1>
                <input type='text' name='message' placeholder='to write something'
                    onChange={
                        (e) => { console.log(e.target.value) }
                    }></input>
            </div>
        )
    }
}

export default EventPractice;
```
# state에 input 값 넣고, 버튼을 누를때 comment 값을 공백으로 설정하고, 값 alert뛰어보기 
```
import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        message: ''
    }
    render() {
        return (
            <div>
                <h1>이벤트 연습</h1>
                <input type='text' name='message' placeholder='to write something'
                    value={this.state.message} onChange={
                        (e) => {
                            this.setState({
                                message: e.target.value
                            })
                        }
                    } />
                <button onClick={
                    () => {
                        alert(this.state.message);
                        this.setState({
                            message: ''
                        });
                    }
                }>CHECK</button>
            </div>
        )
    }
}

export default EventPractice;
```
# 주의! 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라 함수 형태의 값을 전달하는 것! 
# 임의의 메서드로 만들기 
```
import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        message: ''
    }

    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleClick = this.handleClick.bind(this);
    }

    handleChange(e) {
        alert(this.state.message)
        this.setState({
            message: e.target.value
        });
    }

    handleClick() {
        alert(this.state.message)
        this.setState({
            message: ''
        })
    }

    render() {
        return (
            <div>
                <h1>Hello React with event</h1>
                <input
                    type='text'
                    name='message'
                    placeholder='writie'
                    value={this.state.message}
                    onChange={this.handleChange}
                />
                <button onClick={this.handleClick}>Click</button>
            </div>
        )
    }

}
export default EventPractice;
```
```
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleClick = this.handleClick.bind(this);
    }
```
함수가 호출될 때 this는 호출부에 따라 결정되고, 클래스의 임의 메서드가 특정 html 요소의 이벤트로 등록되는 과정에서 메서드와 this의 관계가 끈킨다. 이때문에 임의 메서드가 이벤트로 등록되어도, this를 컴포넌트 자신으로 제대로 가르키 위해서는 메서드를 this와 바인딩 하는 작업이 필요. 만약 바인딩하지 않으면 undefined를 가르킨다. 

# arrow fucntion으로 생성자 메서드에서 메서드 바인딩을 안하는 방법. 
메서드 바인딩은  생성자 메서드에서 하는 것이 정석. 하지만 새 메서드를 만들때마다 constructor도 수정해야 함. 바벨의 transform-class-properties 문법을 사용하여 화살표 함수 형태로 메서드를 정의하는 것. 
```
import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        message: ''
    }

    handleChange = (e) => {
        alert(this.state.message)
        this.setState({
            message: e.target.value
        });
    }

    handleClick = () => {
        alert(this.state.message)
        this.setState({
            message: ''
        })
    }

    render() {
        return (
            <div>
                <h1>Hello React with event</h1>
                <input
                    type='text'
                    name='message'
                    placeholder='writie'
                    value={this.state.message}
                    onChange={this.handleChange}
                />
                <button onClick={this.handleClick}>Click</button>
            </div>
        )
    }

}
export default EventPractice;
```
# 다수의 input 관리하는 방법
```
import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        username: '',
        message: ''
    }

    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        });
    }

    handleClick = () => {
        alert(this.state.username + ':' + this.state.message)
        this.setState({
            username: '',
            message: ''
        })
    }

    render() {
        return (
            <div>
                <h1>Hello React with event</h1>
                <input
                    type='text'
                    name='username'
                    placeholder='username'
                    value={this.state.username}
                    onChange={this.handleChange}
                />
                <input
                    type='text'
                    name='message'
                    placeholder='writie'
                    value={this.state.message}
                    onChange={this.handleChange}
                />
                <button onClick={this.handleClick}>Click</button>
            </div>
        )
    }

}
export default EventPractice;
``` 
객체 안에서 key를 []로 감싸면 그 안에 넣은 레퍼런스가 가르키는 실제값이 key값으로 사용된다.

```
const name = 'variantKey';
const object = {
    [name] = 'value'
};// {'variantKey : 'value'}
```
# onKeyPress 이벤트 핸들링 
 ```
 import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        username: '',
        message: ''
    }

    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        });
    }

    handleClick = () => {
        alert(this.state.username + ':' + this.state.message)
        this.setState({
            username: '',
            message: ''
        })
    }

    handleKeyPress = (e) => {
        if (e.key === 'Enter') {
            this.handleClick();
        }
    }



    render() {
        return (
            <div>
                <h1>Hello React with event</h1>
                <input
                    type='text'
                    name='username'
                    placeholder='username'
                    value={this.state.username}
                    onChange={this.handleChange}
                />
                <input
                    type='text'
                    name='message'
                    placeholder='writie'
                    value={this.state.message}
                    onChange={this.handleChange}
                    onKeyPress={this.handleKeyPress}
                />
                <button onClick={this.handleClick}>Click</button>
            </div>
        )
    }

}
export default EventPractice;
```


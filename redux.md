# 이름에 대한 불변함 : const vs var 
const는 누군가가 이미 만들어진 변수에 재할당 시키려고 할 때, 에러로 알려준다는 점에서 const를 쓰는 것. 

# redux
예측가능한 상태의 저장소 
single source of Truth 
state는 하나의 객체.  한 곳에 데이터를 중앙집중하여서 데이터를 효율적으로 관리할 수 있음. 
dispatcher, reducer을 통해서 state를 바꾸는 것. 직접 바꿀 수 없음. 
getState를 통해 가져갈 수 있음. 
예기치 못하게 가져가는 것을 막는 것. 
UNDO, REDO를 쉽게 할 수 있다. 각각의 스테이트 값들을 생성할 때 철저하게 통제하고 그리고 데이터를 만들 때 원본을 바꾸는 것이아니라 원본을 복제하고, 복제한 데이터를 수정해서 그것을 새로운 원본으로 만드는 이런 방식을 채택하는 것. 변화가 발생되더라도, 서로에게 영향을 주지 않아 독립된 형태를 유지할 수 있고, 이를 홀용하여 UNDO,REDO와 같은 어플리케이션의 상태를 바꾸는게 쉬워진다. 리덕스를 이용하게 되면 현재 상태 뿐만 아니라 이전의 상태까지도 꼼꼼하게 코딩하는 것을 통하여 과거에 이러난 변화로 돌아가서, 그 시점에서 이 어플리케이션의 상태가 어땠는지를 알 수 있고, 해결하는데 도움이 된다. 


# summary
store를 만들면 내부적으로 스테이트 값이 생기고, 스테이트값을 가져올때 getState를 사용해야 하고, 
state 값을 만들어 주기 위해서 reducer를 사용한다. 리듀서 안에서 동작할 때, 만약 state값이 undefined일 경우,
초기화를 위해서 최초로 실행되는 reducer 호출이다. reducer 호출 시 원하는 초기값을 return에 넣어주면 초기값이 저장된다. 

state값을 변경시키는 방법 =reducer가 하는 일
먼저 action이 일어나면 스토어가 dispatch가 리듀서 호출 -> 스테이트 값이 변함.
    ->모습을 바꾸기 위해 subscribe(render)를 등록해서, 스테이트가 바뀔때마다 render를 호출해서 바꿈. 
state를 바꾸기 위해서는 action이 필요하다. 그리고 그것을 dispatch에게 제출하면  디스패치가 리듀서를 호출. 
이전의 스테이트 값과 액션의 값을 동시에 준다. 리듀서 함수가 그것을 분석해서, 스테이트 값의 최종적인 값을 return하면 된다. 
객체를 직접적으로 바꾸면 안되고, redo, undo,시간 여행등을 사용할 수 없게됌. 그러므로 
Object.assign({},state,{color:'red'})을 통해 바꿔주어야함. 
assign의 경우 처음 매개변수 값으로는 {} 값을 주어야 한다. assign의 return값은 첫번째 값이기 때문에. 

1. 상태가 바뀔때 스토어에 dispatch해주면 된다. store.dispatch({type:'CHANGE_COLOR', color:'red'})
2. 어떤식으로 업데이트할지 function red(){}와 같이 지정해 놓고, 
3. 스토어에 구독을 시켜 놓으면  store.subscribe(red); 스테이트가 변경될때 마다 통보를 받아서
그 때마다 자신의 모양을 바꾸어 준다. 

즉 리덕스를 사용하는 이유는, 블루, 레드, 그린이 실행될때마다 각각에 연결되어 있거나 의존관계가 필요가 없고, 
그래서 하나를 수정하면 ,다른 부분도 수정해야하는 문제를 해결할 수 있고, 하나의 store안에 집중하면 되기 때문에, 
연결성과 의존성을 낮출 수 있다. 독립되어 있다는 점에서, redux를 사용하는 이유. 순수성을 유지하는 것. 
불변성에서 리액트와 리덕스가 사용하는 이유. 스테이트가 바뀔때마다 서로 완전 독립된 데이터라는 점. 이러한 독립성을 통해
시간여행도 가능함. 
# 흐름
state에는 직접적으로 변경하거나 접근할 수 없다. reducer로 리덕스를 만들고, dispatch, subscribe,getState를 통해 스토어도 함수들을과 렌더가 협력해서 어플리케이션을 만든다. 
# redux create
```
function reducer(oldState,action){
    //....
}
var store = Redux.createStore(reducer); 
//파라미터롤 reducer function을 주어야 함 .
```
# render
```
function render(){
    var state = store.getState();
    //...
    document.querySelector('#app).innerHTML = `<h1> WEB </h1>`
}
```

html을 통해서 state값을 이용해서 웹페이지를 만든 다음에 실행하면 실제로 ui가 된다. 렌더는 내부적으로 이 스토어에서 데이터를 가져오고, getstate는 state의 값을 가져오가 이제 렌더에게 던져주는 것. 
즉! 렌더는 state 값을 참조해서 UI를 만드는 것. 
만약 렌더함수를 호출하여, 스테이트 값이 변할 때마다 UI가 갱신하도록 하고 싶다.
그러면 subscribe를 사용하면 된다. subscribe에 reder함수를 등록하면, 스테이트 값이 바뀔 대마다 렌더 함수가 호출되어서 새롭게 갱신된다. 
```
store.subscribe(render);
```
# action 

```
function reducer(state,action){
    if(action.type === 'create'){
        var newContents = oldState.contents.concat();
        var newMaxId = oldState.maxId+1;
        newContents.push({id:newMaxId, title:action})
        return Object.assign({},state,{
            contents:newContents,
            maxId:newMaxId,
            mode:'read',
            selectedID:newMaxId
        })
    }
}
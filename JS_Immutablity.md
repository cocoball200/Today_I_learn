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
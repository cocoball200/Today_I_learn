# 라우팅, 라우터란? 
사용자가 어떤 주소로 들어 왔을 때 그 주소에 해당되는 적당한 페이지를 사용자에게 보내주는 것

# BrowseRouter 
리액트 라우터 돔을 적용하고 싶은 컴포넌트의 최상위의 컴포넌트를 감싸주는 wrapper 컴포넌트 
브라우저 라우터라는 컴포넌트로 App을 깜싸준다 
App은 브라우저 컴포넌트 안에서 사용할 수 있게 됨. 
```
 <BrowserRouter><App/> <BrowserRouter/>
 ```
# Route
url에 따라 달라져야 되는 컴포넌트로 감싸 주면 된다. 
```
<Route path='/'><Home></Home></Route>
<Route path='/topics'><Topics></Topics></Route>
<Route path='/contact'><Contact></Contact></Route>
```
 # 문제점. 
 /home으로 들어오면 Home 나오지만, /topics로 들어오면 Home페이지와, Topics 페이지가 같이 나옴. 
 /contact로 들어오면 Home페이지와 contact 페이지가 나옴. 왜냐하면 /에 걸리기 때문. 

 # 해결방법  'exact'
 ```
 <Route exact path='/'><Home></Home></Route>
<Route exact path='/topics'><Topics></Topics></Route>
<Route exact path='/contact'><Contact></Contact></Route>
```

# 만약 동일한 페스가 있으면 두번출력. => 동적 라우팅 
 ```
<Route exact path='/topics'><Topics></Topics></Route>
<Route exact path='/topics'><Topics></Topics></Route>
 ```

 # 다른방법 Switch 
 /에 exact를 거는 이유는 먼저 걸리는 것만 출력하기 때문에 밑에 있는 페스는 걸릴 수가 없음. 그래서 exact를 넣어서, 정확한 값이 들어오면 넘어가도록. 
 ```
 <Switch>
 <Route exact path='/'><Home></Home></Route>
<Route path='/topics'><Topics></Topics></Route>
<Route path='/contact'><Contact></Contact></Route>
 <Switch>

# 없는 페이지로 들어올 경우 hoho
 <Switch>
 <Route exact path='/'><Home></Home></Route>
<Route path='/topics'><Topics></Topics></Route>
<Route path='/contact'><Contact></Contact></Route>
<Route path = '/'>Not found<Route>
 <Switch>
 ```

# link 컴포넌트 
href를 쓰면 클릭할때 마다 매 페이지가 로딩된다. 그래서 실제 동적으로 가져오는 데이터는 코딩으로 만들거나 ajax와 같은 걸로 통해서 비동기적으로 데이터를 끌어와서 처리하곤 했는데, 하지만 link 컴포넌트를 사용하면 페이지가 새로 로딩을 안해도 됨. 
```
<ul>
    <li><Link to ='/'></link></li>
    <li><Link to ='/topics'>Topics</link></li>
    <li><Link to ='/contact'>Contact</link></li>
</ul>
```
# NavLink
현재 사용자가 어디에 있는지 표시할때 유용하다. NavLink를 사용하면, class="active"가 추가 되어서, .active 로 스타일값도 줄수 있음 마치 hover처럼. 짱짱맨.
```
<ul>
    <li><NavLink to ='/'></NavLink></li>
    <li><NavLink to ='/topics'>Topics</NavLink></li>
    <li><NavLink to ='/contact'>Contact</NavLink></li>
</ul>
```

# Switch없이 동적으로 짧은 코드로 해결 하는 방법. 
Nested Routing , Param , useParam
1. 링크에 넣을 컨텐츠 생성
2. 컨텐츠의 내용을 보여주기 위해서 빈 리스트를 생성하고, 거기에 <li>를 넣어주고, return해서 뿌림 
```
    var lis = [];
    for(var i=0; i<contents.length; i++){
        lis.push(<li key={contents[i].id}><NavLink to={'/topics/'+contents[i].id}>{contents[i].title}</NavLink></li>)

    return(
        <div>
            <h2>Topics</h2>
            <ul>
                {lis}
            </ul>
    )
    }
``` 
3. 링크마다 새로운 topics/ 뒤에 새로운 id 값을 주기 위해서  /:topic_id 
```
return (
        <div>
            <h2>Topics</h2>
            <ul>
                {lis}
            </ul>
            <Route path="/topics/:topic_id">
                <Topic></Topic>
            </Route>
        </div>
    )
```

4. useParam을 이용해 자동으로 topic_id를 가져 올 수 있다. 
```
function Topic(){
    var params = useParams();
    var topic_id = params.topic_id;
    var selected_topic = {
        title:'Sorry',
        description:'Not Found'    
    }
    for(var i=0; i<contents.length; i++){
        if(contents[i].id === Number(topic_id)){
            selected_topic = contents[i];
            break;
        }
    }
    return (
        <div>
            <h3>{selected_topic.title}</h3>
            {selected_topic.description}
        </div>
    );
}
```

# 전체코드, 참조 생황코딩 react-router 
라우터가 헷갈릴때 꾸준히 보기! 
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import * as serviceWorker from './serviceWorker';
import {BrowserRouter, Route, Switch, Link, NavLink, useParams} from 'react-router-dom';

function Home(){
    return (
        <div>
            <h2>Home</h2>
            Home...
        </div>
    )
}

var contents = [
    {id:1, title:'HTML', description:'HTML is ...'},
    {id:2, title:'JS', description:'JS is ...'},
    {id:3, title:'React', description:'React is ...'},
]

function Topic(){
    var params = useParams();
    var topic_id = params.topic_id;
    var selected_topic = {
        title:'Sorry',
        description:'Not Found'    
    }
    for(var i=0; i<contents.length; i++){
        if(contents[i].id === Number(topic_id)){
            selected_topic = contents[i];
            break;
        }
    }
    return (
        <div>
            <h3>{selected_topic.title}</h3>
            {selected_topic.description}
        </div>
    );
}

function Topics(){
    var lis = [];
    for(var i=0; i<contents.length; i++){
        lis.push(<li key={contents[i].id}><NavLink to={'/topics/'+contents[i].id}>{contents[i].title}</NavLink></li>)
    }
    return (
        <div>
            <h2>Topics</h2>
            <ul>
                {lis}
            </ul>
            <Route path="/topics/:topic_id">
                <Topic></Topic>
            </Route>
        </div>
    )
}
function Contact(){
    return (
        <div>
            <h2>Contact</h2>
            Contact...
        </div>
    )
}

function App(){
    return (
        <div>
            <h1>React Router DOM example</h1>
            <ul>
                <li><NavLink exact to="/">Home</NavLink></li>
                <li><NavLink to="/topics">Topics</NavLink></li>
                <li><NavLink to="/contact">Contact</NavLink></li>
            </ul>
            <Switch>
                <Route exact path="/"><Home></Home></Route>
                <Route path="/topics"><Topics></Topics></Route>
                <Route path="/contact"><Contact></Contact></Route>
                <Route path="/">Not found</Route>
            </Switch>
        </div>
    )
}

ReactDOM.render(<BrowserRouter><App /></BrowserRouter>, document.getElementById('root'));
```


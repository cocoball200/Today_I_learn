# 라우팅, 라우터란? 
사용자가 어떤 주소로 들어 왔을 때 그 주소에 해당되는 적당한 페이지를 사용자에게 보내주는 것

# BrowseRouter 
리액트 라우터 돔을 적용하고 싶은 컴포넌트의 최상위의 컴포넌트를 감싸주는 wrapper 컴포넌트 
브라우저 라우터라는 컴포넌트로 <BrowserRouter><App/> <BrowserRouter/>을 깜싸준다 
App은 브라우저 컴포넌트 안에서 사용할 수 있게 됨. 
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

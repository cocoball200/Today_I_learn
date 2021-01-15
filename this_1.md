# this는 왜 쓰는 걸까? 
```
function checkName(){
    return this.name.toUpperCase();
}
function helloGreeting(){
    var message = "안녕~!" +checkName.call(this);
    console.log(message);
}

var mia = {
    name : "mia"
};

var james = {
    name:"james"
};

checkNmae.call(mia);
checkName.call(james);

helloGreeting.call(mia);
helloGreeting.call(james)
```
암시적인 객체 레퍼런스를 함께 넘기는 this 체계가 API 설계상 좀 더 깔끔하고 명확하며 재사용하기 쉽다. 
사용 패턴이 복잡해질수록 보통 명시적인 인자로 콘텍스트를 넘기는 방법이 this 콘텍스느를 사용하는 것 보다 더 지저분해짐. 

```
function foo(num){
  console.log("foo : "+ num); //"foo : 6" ,"foo : 7" ,"foo : 8" ,"foo : 9"
  //foo가 몇번 호출됐는지
  this.count++;
}

foo.count = 0;

var i; 

for (i=0; i<10; i++){
  if(i>5){
    foo(i);
  }
}

console.log(foo.count) // 0
```
분명 foo에 6,7,8,9로 호출 횟수가 표시되었다. 하지만 foo.count는 0; foo.count = 0 은 foo라는 함수객체에 count라는 프로퍼티가 추가된 것. this.count랑 다른 것. 여기서 this는 함수 객체를 바라보는 것이 아님. 

```
function foo(){
    foo.count = 4;  //foo는 자기 자신을 가리킨다. 
}
setTimeout(function(){
    //익명함수는 자기 자신을 가리킬 방법이 없음 
},10)
```
이름 붙은 함수인 foo는 foo라는 함수명 자체가 내부에서 자신을 가리키는 레퍼런스로 쓰인다. 하지만 setTimeout()에 콜백으로 전달한 함수는 이름 식별자가 없으므로 함수 자신을 참조할 방법이 없다. 

```
function foo(num){
  console.log("foo : "+ num);
  //foo가 몇번 호출됐는지
  this.count++;
}

foo.count = 0;
var i; 

for (i=0; i<10; i++){
  if(i>5){
    foo.call(foo,i);
  }
}

console.log(foo.count);  //4 

```
this는 foo를 어떻게 호출하느냐에 따라 달라지기 때문에. foo.call(foo,i)를 통해 할 수 있음. 

# this
this는 런타임 시점에 바인딩 되며 함수 호출 당시 상황에 따라 콘텍스트가 결정된다. 
함수 선언 위치와 상관없이 this 바인딩은 오로지 어떻게 함수가 호출했느냐에 따라 달라진다. 
함수를 호출하면 실행 콘텍스트가 만들어진다. 여기엔 함수가 호출된 근원과 호출방법, 전달된 인자 등의 정보가 담겨있다. this 레퍼런스는 그중 하나로, 함수가 실행되는 동안 이용할 수 있다. 

```
function baz(){
  //호출 스택: 'baz'
  //따라서 호출부는 전역 스코프 내부다.
  console.log("baz");
  bar();
}

function bar(){
  //호출 스택: "baz" -> "bar"
  //호출부는 "baz" 내부다.
  console.log("bar");
  foo();
}
function foo(){
  //호출 스택: "baz" -> "bar" -> "foo"
  //따라서 호출부는 bar 내부다. 
  console.log("foo");
}
baz(); // baz의 호출부 
```

# this 규칙 

# 기본 바인딩 
```
function foo(){
  console.log(this.a);
}
var a = 2;
foo();
``` 
var a =2는 전역 스코프에 변수를 선언되어 있다. 전역 스코프에 변수를 선언하고, 변수명과 같은 이름의 전역 객체 프로퍼티가 생성된다. 그래서 foo() 함수 호출 시 this.a는 전역객체 a다. 기본 바인딩이 적용되어 this는 전역 객체를 참조한다. 하지만 스트릭 모드에서는 undefined가 뜬다. 스트릭 모드에서는 전역 객체가 기본 바인딩 대상에서 제외하기 때문이다. 

# 암시적 바인딩 
호출부에 콘텍스트 객체가 있는지, 즉 객체의 소유/포함 여부를 확인하는 것이다. 
```
function foo(){
  console.log(this.a);
}
var obj = {
  a:2,
  foo:foo
}
obj.foo();

```

obj.foo(); 함수를 obj에서 프로퍼티로 참조하고 있다. foo()를 처음부터 foo 프로퍼티로 선언하든 이 예제처럼 나중에 레퍼런스로 추가하든 obj 객체가 이 함수를 정말로 소유하거나 포함하는 것은 아니다. 그러나 호출부는 obj 콘텍스트로 foo()를 참조하므로  * obj 객체는 함수 호출 시점에 함수의 레퍼런스를 소유하거나 포함한다고 볼 수 있다. *
foo() 호출 시점에 이미 obj  객체 레퍼런스는 준비가 되었고, 함수 레퍼런스에 대한 콘텍스트 객체가 존재할 때 암시적 바인딩 규칙에 따라 이 콘텍스트 객체가 함수 호출 시 this에 바인딩 된다. foo()호출 시 obj는 this이니 this.a는 obj,a가 된다. 

```
function foo(){
  console.log(this.a);
}

var obj2 = {
  a:42,
  foo:foo
}
var obj1 = {
  a:2,
  obj2:obj2
}
obj1.obj2.foo();

```
위의 예제처럼 객체 프로퍼티 참조가  체이닝된 형태라면 최상위/최하위 수준의 전보만 호출부와 연관된다. 

# 암시적 소실 
```
function foo(){
  console.log(this.a);
}

var obj = {
  a:2,
  foo:foo
};

var bar = obj.foo;
var a = "이건 전역";
bar(); //"이건 전역" 
```
bar는 obj의 foo를 참조하는 변수처럼 보이지만 실은 foo를 직접 가리키는 또 다른 레퍼런스다. 그리고 호출부에서 그냥 평범하게 bar() 호출하여서 기본 바인딩 적용. 암시적 바인딩은 obj. 으로 해야. 

# 명시적 바인딩. 
call(), apply() 메서드를 이용한다. 두 메서드는 this에 바인딩 할 객체를 첫째 인자로 받아 함수 호출 시 이 객체를 this로 세팅한다. this를 지정한 객체로 직접 바인딩 하므로 이를 명시적 바인딩이라 한다. 
```
function foo(){
  console.log(this.a); //2 
}

var obj = {
  a:2,
};

foo.call(obj); 
```
foo.call()에서 명시적으로 바인딩 하여 함수를 호출하므로 this는 반드시 obj가 된다. 하지만 명시적으로 바인딩하더라도 this 바인딩이 도중에 소실되거나 프레임워크가 임의로 덮어써 버리는 문제는 해결할 수 없다. 

```
function foo(el){
  console.log(el,this.id);
}

var obj = {
  id : "obj"
};

[1,2,3].forEach(foo,obj) // 1 obj, 2 obj 3 obj
```

# new 바인딩
자바스크립트에서 생성자의 정의를 내려보면. 자바스크립트 생성자 앞에 new 연산자가 있을 때 호출되는 일반 함수에 불과하다. 클래스에 붙은 것도 아니고 클래스 인스턴스화 기능도 없다. 심지어 특별한 형태의 함수도 아니다. 단지 new를 사용하여 호출할 때 자동으로 붙들려 실행되는 그저 평범한 함수이다. 

함수앞에 new를 붙여 생성자를 호출하면 새 객체가 만들어지고, 새로 생성된 객체의 [[Protortype]]이 연결되고, 새로운 생성된 객체는 해당 함수 호출 시 this로 바인딩 된다. 이 함수가 자신의 또 다른 객체를 반환하지 않는한 new와 함께 호출된 함수는 자동으로 새로 생성된 객체를 반환한다. 즉 new는 함수 호출 시 this를 새 객체와 바인딩하는 방법이다. 

# 명시적 바인딩과 암시적 바인딩의 우선순위 
```
function foo(){
  console.log(this.a); 
}

var obj1 = {
  a:2,
  foo:foo
};
var obj2 = {
  a:3,
  foo:foo
};

obj1.foo(); //2
obj2.foo(); //3 

obj1.foo.call(obj2); //3 
obj2.foo.call(obj1); //2 
```
코드에서 본 것처럼 명시적 바인딩이 우선순위가 더 높다 . 암시적 바인딩 보다 .


# this 무시 
call, apply, bind 메서드에 첫 번째 인자로 null 또는 undefined를 넘기면 this 바인딩이 무시되고 기본 바인딩 규칙이 적용 

```
function foo(){
    console.log(this.a);
}
var a = 2;
foo.call(null) //2 
```
null 같은 값으로 this 바인딩을 하는 이유는 apply()는 함수 호출 시 다수의 인자를 배열 값으로 죽 펼쳐보내는 용도로 자주 쓰인다. bind()도 유사한 방법으로 인자들을 커링하는 메서드로 많이 사용한다. 
apply,bind 모두 반드시 첫 번째 인자로 this 바인딩을 지정해야 함으로. 
```
function foo(a,b){
  console.log("a : " + a + " b : " + b); 
}

foo.apply(null,[2,3]); //"a : 2 b : 3"
var bar  = foo.bind(null, 5);
bar(9); //"a : 5 b : 9"

```
option+ o 를 ø로 변수를 만들어서 빈객체를 넣는 것도 있다. null값이 들어가면 내부적으로 this를 레퍼런스를 참조하면 기존 바인딩이 적용되어 전역 변수를 참조하거나 최악으로 변경하는 예기치 못한일 이 있음으로. 아래의 방법처럼 빈 객체를 만드는 것도 방법. 
```
function foo(a,b){
  console.log("a : " + a + " b : " + b); 
}

var ø = Object.create(null);

foo.apply(ø,[2,3]);
var bar  = foo.bind(ø, 5);
bar(9);

```
# ES6 화살표 함수 
화살표 함수는 표준 바인딩 규칙을 무시하고 렉시컬 스코프로 this를 바인딩한다. 즉 에두른 함수 호출로부터 어떤 값이든 this 바인딩을 상속한다. 

# 정리
함수 실행에 있어서 this 바인딩은 함수의 직접적인 호출부에 따라 달라진다. 일단 호출부를 식별한 후 다음 4가지 규칙을 열거한 우선순위에 따라 적용된다.
1. new로 호출했다면 새로 생성된 객체로 바인딩 된다.
2. call 이나 apply 또는 bind로 호출됐다면 주어진 객체로 바인딩 된다. 
3. 호출의 주체인 콘텍스트 객체로 호출했다면 바로 이 콘텍스트가 객체로 바인딩 된다.
4. 기본바인딩에서 염격모드는 undefined, 그 밖엔 전역 객체로 바인딩 된다. 


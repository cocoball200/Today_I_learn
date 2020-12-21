# 원시타입 
자바스크립트는 7개의 내장타입을 가짐. object을 제외한 나머지는 원시타입.
null, undefined, boolean, number, string, object, symbol 

하지만 null의 경우 typeOf를 찍으면 object이 나옴. 이건 자바스크립트의 설계상의 오류. 
즉 null은 falsy한 유일한 원시 값이면서도, 타입은 'object'인 특별한 존재. 
```
var a = null;
!a && typeof a === 'object' //true
```
typeof는 이 변수에 들어가 있는 값의 타입이 무엇이지 알려주는 연산자. 반환하는 값은 항상 string임! 
```
typeof typeof 24 // "string"
```

배열 또한 객체이다. 배열은 숫자 인덱스를 가지며, length 프로퍼티가 자동으로 관리되는 등의 추가 특성을 지니는 객체의 '하위타입' 
```
typeof [1,3,2] === 'object' //true
```

# function 
function은 object의 하위타입이다. 함수는 호출 가능한 개체라고 명시되어있다. 

# 변수는 타입이 없다.
값에는 타입을 갖고 있지만, 변수에는 따로 타입이 없다. 변수는 언제라도, 어떤 형태의 값으로 가질수 있다. 
즉 변숫값이 처음에 할당된 값과 동일한 타입을 가질 필요가 없다. 이 점이 자바와 다른점인듯. 

# undefined
```
var name;
name;  //undefined
age;  //ReferenceError: age가 정의 되지 않았습니다. 
```
그런데, typeof를 하면 결과가 똑같이 undefined임. 
```
typeof name // "undefined"
typeof age // "undefined"
```
분명 age는 선언하지 않았는데, typeof age는 "undefined"이다. 이러한 typeof를 통해 변수 선언 여부 체크를 할 수 있음. 
```
if(typeof atob === 'undefined'){
    atob = function(){}
}
```
atob에 var을 빼는 이유는, if var atob으로 선언하면(전역 atob은 이미 존재함으로) , 선언 자체가 최상위 스코프로 호이스팅된다. 이렇게 특수한 타입의 전역 내장 변수를 중복 선언하면 에러를 던지는 브라우저가 있을 수 있다. 명시적으로 var을 빼야 선언문이 호이스팅 되지 않는다. 

# typeof를 활용하여 변수가 정의되어 있는지 이미 있는지 확인. 
```
(function(){
    function a(){}

function foo(){
    var validation = 
    (typeof a !== 'undefined')?
    a: //그대로 사용. 
    function(){
        //함수를 여기다가 정의. 왜냐면 a가 undefined이기 때문에 
    }

    var val = validation()
}
 foo();
})();
```
참고: YOU DON'T KNOW JS


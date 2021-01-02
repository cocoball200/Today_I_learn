# 강제변환
어떤 값을 다른 타입의 값으로 바꾸는 과정이 명시적이면 '타입 캐스팅', 암시적이면 '강제 변환'이다. 
타입캐스팅은 정적 타입 언어에서 컴파일 시점에, '강제변환'은 동적 타입 언어에서 런타임 시점에 발생한다. 
명시적 강제변환은 의도적으로 타입변환을 일으킨다는 사실이 명백하고, 암시적 강제변환은 다른 작업 도중 불분명한 부수 효과로부터 발생하는 타입변환이다. 

```
var a= 42;
var b = a + "" //암시적 강제변환
var c  = String(a); //명시적 강제변환. 

```
# (~) 틸드 
자바스크립트 코드를 보면 ~랑 |가 있는데 ||랑은 다른 것 같은데, 무슨의미지 🤷🏻‍♀️라고 생각했다. | 와 ~ 비트 연산자를 적용하면 전혀 다른 숫자 값을 생성하는 강제변환 효과가 있다. 뭘 까 틸드~!!! 책의 예제를 잘 이해가 안됨으로, 좀 더 공부해서 추가할 것.!!! 
# Falsy 객체 
undefined, numm, false, 0 , -0, NaN , ""  falsy 값이며, 불리언으로 강제 변환하면 false. 
```
var a = new Boolean(false); 
var b = new Number(0);
var c = new String("")
var d = Boolean(a && b && c);
d; //true
```

```
var d = a && b && c
```
값은 ""이다. &&는 a와 b를 비교하여 true값을 갖고 있있는 것을 찾고, 또 다시 true값이 있는 값을 끝까지 찾아가기 때문. 

자바스크립트 개발 시 불리언 값으로 명시적인 강제변환을 할때 !! 이중 부정 연산자를 사용한다 
! 부정 단항 연산자는 표현식이 true이면 false, false이면 true로 결과를 반대로 뒤집기 때문에 불리언 타입으로 변환되는 것, 반면에 한번 더 부정하면 원래 표현식의 true,fasle 값을 얻게 되는 것. 

```
var a = "0";
var b = [];
var c = {};

var d = "";
var e = 0;
var f = null;
var g;

console.log(Boolean(a)) //true
console.log(Boolean(b)) //true
console.log(Boolean(c)) //true

console.log(Boolean(d)) //false
console.log(Boolean(e)) //false
console.log(Boolean(f)) //false
console.log(Boolean(g)) //false
```

```
var a = "0";
var b = [];
var c = {};

var d = "";
var e = 0;
var f = null;
var g;

console.log(!!a);  //true
console.log(!!b); //true
console.log(!!c); //true

console.log(!!d); //false
console.log(!!e); //false
console.log(!!f); //false
console.log(!!g); //false
```

# && 와 || 연산자 
||(논리 OR) 및 &&(논리 AND) 연산자. 
&&,|| 연산자의 결괏값이 반드시 불리언 타입이어야 하는 것은 아니며 항상 두 피연산자 표현신 중 어느 한쪽 값으로 귀결된다. 

```
var a = 33;
var b = "lol";
var c = null;

console.log(a || b); //33 
console.log(a && b); //lol

console.log(c|| b); // lol
console.log(c && b); //null
```
||연산자와 && 연산자는 우선 첫번째 피연산자의 불리언 값을 평가한다. 피연사자가 비 불리언 타입이면 강제변환 후 평가를 하고, || 연산자는 그 결과가 true이면 첫 번째 피 연산자 a,c값을, false면 두번재 피연자 b값을 반환한다. 

&&연산자는 true면 두 번째 피연산자의 b의 값을, false 이면 첫번째 피연산자 (a,c)값을 반환한다. 
넘나 헷갈리는 것 ㅠ 계속 보아야지. 저번에도 공부했는데, 막상 문제풀면 헷갈린다 ㅠ.. 

```
function foo(a,b){
  a = a || "hello, ";
  b = b || "Mia";
  console.log(a +" "+ b);
}

foo(); // "hello, Mia"   왜냐면 파라미터로 아무 것도 들어가지 않음으로 false 
foo("와 너무 ","햇갈리는걸 "); // "와 너무 햇갈리는걸"
```
```
function foo(){
  console.log("a가 truthy일 때 두번째 피연산자를 선택해서 foo()함수가 나옴");
}

var a = 22;
a && foo();
```
&&연산자는 첫 번째 피연산자의 결과가 truthy일 때에만 두 번째 피 연산자를 선택한다고 했는데, 이러한 특성을 가드 연산자라고 하고, 첫 번째 표현식이 두번째 표현식의 가드 역할을 하는 것이다. a 평가 결과가 truthy일 때에만 foo()가 호출된다. 평가 결과가 falsy이면 a && foo() 표현식은 그 자리에서 실행을 멈추고, foo()는 호출 되지 않는다.
```
function foo(){
  console.log("a가 truthy일 때 두번째 피연산자를 선택해서 foo()함수가 나옴");
}

var a =  null;
a && foo();
``` 
아무것도 안나옴. 
```
var a = 22;
var b = null;
var c = "foo";
if(a && (b ||c)){
  console.log("It's working")
}
```
a && (b ||c)  
우선 a값이 truthy 값임으로, (b ||c)를 선택하고, b값이 falsy값이여서 c를 선택한다. c는 "foo"임으로, truthy이기때문에, if문에 만족하여, it's working이 찍힌다. 사실 a && (b ||c) 의 값은 "foo"이다. 하지만 if 문은 이 "foo"를 불리언 타입으로 강제변환하여 true로 만든다. if를 사용하면서 모르게 암시적 강제변환을 사용하고 있었던 것! 

# 느슨한/엄격한 동등 비교 
느슨한 동등 비교는 == 연산자를, 엄격한 동등 비교는 === 연사자를 각각 사용한다. 
==는 동등함의 비교시 강제변환를 허용하지만, 
===는 강제변환을 허용하지 않는다. 
즉, 강제변환이 필요하다면 느슨한 동등연산자 (==)를, 강제형변환이 필요하지 않다면 엄격한 동등한 연산자(===)를 사용한다. 

과거에 동등비교를 공부할 때, 이부분이 정말 이해할 수 없었다. 왜 false인가. 물론 자바 공부했을때는 당연히 false라고 생각했겠지만, 자바스크립트 형변환 공부하고 난 후에는, 아니 왜 ==는 강제 형 변환이 일어나니까, true아닌가?라고.. a도 truthy, b도 truthy값이니까. 
```
var a = true;
var b = "22";
console.log(a == b);
``` 
Type(a)은 불리언이고, ToNumber(a) -> 1로 강제 변환된다. 따라서 1 == "22"이 되는데 타입이 다르므로, "22"는 22로 바뀌어서 1 == 22가 된다. 그래서 false이다. 
"22" == true, false 둘다 아니다.... 
즉 내가 생각했던 순서가 실제의 구현된 알고리즘의 순서와 달랐던 것.
"22"는 truthy 값이지만 "22"가 불리언인 true값으로 강제변환이 되는 것이 아니라, true가 1로 강제 변환된고, 그 후 "22"가 22로 변환되는 것.
즉 == 의 피연사자 한쪽이 불리언 값이면 예외 없이 그 값이 먼저 숫자로 강제변환된다. 
그러므로 true, == false 같은 코드를 쓰면 안된다 ㅎㅎ.. 

참고: YOU DON'T KNOW JS 
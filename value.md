# 배열
자바스크립트 배열은 자바처럼 타입이 엄격하지 않다. 문자열, 숫자, 객체, 심지어 다른 배열이나 어떤 타입의 값이더라도 담을 수 있다.
또한 배열 크기는 미리 정하지 않아도 된다. 
배열 값에 delete 연산자를 적용하여 슬롯을 제거할 수 있지만, 마지막 원소까지 제거해도 length 프로퍼티 값까지 바뀌지 않는다는 점 주의해야 한다. 
```
var array = [1, undefined, null, [1,2,3], {name:'mia'}];

console.log(array);
//[1, undefined, null, [1, 2, 3], [object Object] {
  name: "mia"
}]
console.log(array.length); //5
console.log(array[2]); //null
console.log(array[4].name); //'mia'

var a = [ ];

a[0] =1;
a[1] = undefined;
a['nationality'] ='Korean'
```
배열값에 undefined를 넣어주려면, 명시적으로 넣어주어야 하면, 배열의 인덱스는 숫자이지만,
배열 자체도 하나의 객체여서 키/프로퍼티 문자열을 추가할 수 있다. 하지만 배열의 length는 중가하지 않음. 

```
var b = [];
b["10"] = 42;
console.log(b.length) //11
````
주의 할 점은 키 값을 10진수 숫자로 문자열로 넣어주면, 배열의 length가 올라간다. 마치 문자열 키가 아닌
숫자 키를 사용한 것 같은 결과 초래. 그래서 위 처럼 사용하는 것 추천하지 않음. 

```

# 유사배열
유사배열값(숫자 인덱스가 가리키는 값들의 집합)을 진짜 배열로 바꾸고 싶을때는, 배열 유틸리티 함수를 이용하여 해결 할 수 있다. 
예로, indexOf(),concat(), forEach() 등. 
DOM 쿼리 작업을 수행하면 비록 배열은 아니지만 변환 용도로는 충분한, 유사 배열 형태의 DOM 원소 리스트가 반환된다. 
함수에서 arguments 객체를 사용하여 인자를 리스트로 가져오는 것도 그 예이다. 

```
function foo(){
  var arr = Array.prototype.slice.call(arguments);
  console.log(arr); // ['2020','lol']
  arr.push("style");
}

foo("2020","lol"); //['2020','lol','style']
```
slice() 함수에 인자가 없으면 기본 인자 값으로 구성된 배열을 복사한다. es6부터는 Array.from()을 통해 배열을 복사할 수 있다. 

```
function foo(){
  var arr = Array.from(arguments);
  console.log(arr);
  arr.push("style");
}

foo("2020","lol");
```
# 문자열 
문자열은 유사배열. 배열과 겉모습이 닮음.
 예를들어, length 프로퍼티, indexOf() 메서드, concat() 메서드를 가진다. 
```
var a = 'foo';
var b = ['f','o','o'];


console.log(a.length); //3
console.log(b.length); //3 

a.indexOf('o'); //1
b.indexOf('o'); //1

var c = a.concat("bar");
var d = b.concat(['b','a','r'])


console.log(a === c); //false
console.log(b === d); //false

console.log(a); //'foo'
console.log(b);//['f','o','o']

```
문자열은 원시타입으로, 불면값임. 
배열값은 object으로, 가변값이다. 
문자열은 불변값이기 때문에, 그 내용을 바로 변경하지 않고, 항상 새로운 문자열을 생성한 후 반환한다. 
반면에 대부분의 배열 메서드는 그자리에서 곧바로 원소를 수정한다. 
 var c  = a.toUppercase()를 해야함. 
새로운 변수에 할당해야함. a 자체에는 변하지 않는다. 

* 문자열을 다룰 때 유용한 대부분의 배열 메서드는 사실상 문자열에서 쓸 수 없지만, 
문자열에 대해 불변 배열 메서드를 빌려 쓸 수 있다.  왜냐하면 문자열은 불변 값이라 바로 변경되지 않기 때문에,
배열의 가변 메서드는 통하지 않고, 빌려쓰는 것도 안된다. 

```
var a = 'foo'; //string

console.log(a.join); // undefined
console.log(a.map); //undefined


var b = Array.prototype.join.call(a,"❣️");
var d = Array.prototype.map.call(a, function(i){
  return i.toUpperCase()+"💖";
}).join("");

console.log(b) //"f❣️o❣️o"
console.log(d) //"F💖O💖O💖"

```
# 문자열을 배열로 바꾸고 원하는 작업을 수행한 후 다시 문자열로 되돌리는 것이 또다른 꼼수이다. Hack
단순 문자열에 대해 좀 지저분 하더라도 빠르게 써먹을 방법이 필요하다면 나쁘지 않지만, 복잡한 문자가 섞여 있는 경우 특수문자나 멀티바이트등 이 방법이 통하지 않음. 이때는 정교한 라이브러리 유틸리티가 필요함. 에스베레르 참고. 
```
var c = a.split("").reverse().join("");
console.log(c) //oof 

```
즉 문자열 자체에 어떤 작업을 빈번하게 수행하는 경우라면, 문자열을 문자 단위로 저장하는 배열로 취급하는 것이 더 나을 수도 있다. 
문자열로 나타내야 할 대는 언제나 문자 배열에 join("") 메서드를 호출하면 된다. 

# 숫자 
자바스크립트는 자바와 달리 숫자 타입은 number가 유일하고, 정수, 부동 소수점 숫자를 모두 아우른다. 
숫자 값은 Number 객체 wrapper로 박싱할 수 있기 때문에, Number.prototype 메서드로 접근할 수 있다. 
# toFixed()
예를들면, toFixed() 메서드는 지정한 소수점 이하 자릿수까지 숫자를 나타낸다.  리턴값 스트링. 
```
var a = 42.59;
a.toFixed(0); //43
a.toFixed(1); //42.6
a.toFIixed(8);//42.59000000
```

# toPrecision() 
toFixed랑 비슷하지만 유효 숫자 개수를 지정할 수 있다. 특이한 건 리턴값이 string임. 
 
```
var a = 42.59; 
a.toPrecision(0) //error 1부터 지정해야함 
console.log(a.toPrecision(1));//"4e+1"
console.log(a.toPrecision(2)); //"43"
console.log(a.toPrecision(7)); //"42.59000"

console.log(a)
```

# 주의사항 
.이 소수점일 겨우엔 프로퍼티 접근자가 아닌 숫자 리터럴의 일부로 해석되므로, .연산자를 사용할때 주의 .
```
42.toFixed(3); //systaxerror

(42).toFixed(3); //"42.000"
0.42.toFixed(3); //"0.420"
42..toFixed(3); //"42.000"

```
42.toFixed(3); //systaxerror 이 에러가 난 이유는 .이 42. 리터럴의 일부가 되어벼러서 .toFixed메서드에 접근 할 수 없음. 
 42..toFixed(3); //"42.000" 앞의 .은 숫자 리터럴의 일부, 두번째는 프로퍼티 연산자로 해석됨. 
하지만 이런 코드는 보기 어색하고 불편하므로 잘 쓰지 않는다. 
# 큰 숫자를 쓸 경우 
var onethousand  = 1E3;
var onemillionehundrenthousand = 1.1E6

# 작은 소수 값. 
0.1 + 0.2 === 0.3 //false 
왜냐면 실제로는 0.30000000004에 가까움으로. 

# Undefined 
Undefined 타입의 값은 undefined만 있다. 값을 아직 가지지 않은 것. 

# void 연산자
표현식 void__는 어떤 값이든 '무효로 만들어' 항상 결과값을 undefined로 만든다. 기존값은 변화가 없고, 연산 후 값은 복구할 수 없음 
```
var a = 42;
console.log(void a, a) // undefined 42
```
# NaN 
수학 연산 시 두 피 연산자가 전부 숫자가 아닐 경우 유요한 숫자가 나올 수 없는 경우 그 결과는 NaN.
유효하지 않은 숫자, 실패한 숫자 또는 몹쓸 숫자. 
```
var a = 2 / "foo" // NaN

typeof a === "number"; //true
```
???? NaN의 typeof 는 숫자임... 

```
a == NaN //false
a === NaN // false
```
롸? NaN은 어떤 NaN과도 동등하지 않다. 즉 자기 자신과도 같지 않고, 사실상 반사성이 없는 x === x로 식별되지 않는 유일무이한 값. 그래서 isNaN()을 사용하애 함. 여기서 결함은 인자 값이 숫자인지 여부를 평가할 뿐이다. 
```
var b = 'foo'
window.isNaN(b)  //true
``` 
b는 string인데...  그래서 ES6는 Number.isNaN()이 등장. 

# 무한대 
자바에서는 0으로 나누기가 안되었음. 하지만 자바스크립트는 0으로 나눌 수 있음.  infinity/-infinity로 결과값을 준다. 

# 특이한 동등 비교. 
```
0 == -0 //true 
0 === 0 //true

```
Object.is()를 사용하면 된다. 
```
var a = 2 / "foo";
var b = -3 * 0;

console.log(Object.is(a,NaN)); //true
console.log(Object.is(b,-0)); //true
console.log(Object.is(b,0)); //false
```
# 레퍼런스. 
```
var a =2;
var b =a;
b++;
console.log(a);
console.log(b);
```
원시형의 타입은 언제나 값-복사 방식으로 할당/전달 되어서, 원본이 바뀌어도, 참조된 값에 영향을 주지 않음. 
하지만 객체는 다름. 
```
var c =[1,3,9];
var d =c;
d.push(11);
console.log(c); //[1,3,9,11]
console.log(d);// [1,3,9,11]
```
c와 d는 모두 합성 값이자 동일한 공유값 [1,3,9]에 대한 개별 레퍼런스. c와 d가 [1,3,9]를 소유하는 것이 아니라, 이 값을 동등하게 참조하는 것이다. 따라서 레퍼선스로 실제 공뷰한 배열 값이 변경되면, 이 공유 값 한군데에만 영향을 미치므로, 두 레퍼런스는 갱신된 값 [1,3,9,11]을 동시에 바라본다. 

덜 피곤할때, 이부분 다시 보기, 코어 자바스크립트 책에서 봤을때는 이해가 됐는데, 조금 부족하게 이해가 된거 같음. 



참고: YOU DON'T KNOW JS 


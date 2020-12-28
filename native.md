# 네이티브는 내장 함수. 
네이티브란 특정 환경( 브라우저 등의 클라이언트 프로그램)에 종속되지 않은 ECMAScript 명세의 내장객체를 말합니다. Window, Button는 네이티브가 아니다. 
String, Number, Boolean, Array, Object , Function , RegExp , Date , Error , Symbol
```
var a = new String('hello, world');
console.log(a.toString()); // 'hello world'
typeof a; //object
a instanceof String //true
Object.prototype.toString.call(a);  //[object String]
console.log(a) //string{'hello world'}
```
즉 new String('hello world')는 'hello world'를 감싸는 래퍼를 생성하며 원시값 'hello world'는 아니다. 

# 객체 래퍼의 함정. 
Boolean
```
var a = new Boolean(false);
if(a!){
    console.log("oops");
}
``` 
위의 결과는 실행되지 않는다. false를 객체 래퍼로 감쌋지만, 객체가 truthy라는 점이다. 안에 들어있는 값이 false 값과 반대라는 점이다. 
객체 래퍼로 직접 박싱하는 것을 추천하지 않음.  직접 객체 형태로 써야할 이유는 거의 없고, 필요시 엔진이 알아서 암시적으로 박싱하게 하는 것이 낫다. 

# 언박싱
객체 래퍼의 원시 값은 valueOf() 메서드로 추출한다. 
``` 
var a = new String('abc');
a.valueOf() // 'abc'
``` 

# 네이티브, 생성자
배열, 객체, 함수, 정규식 값은 리터럴 형태로 생성하는 것이 일반적이지만, 리터럴은 생성자 형식으로 만든 것과 동일한 종류의 객체를 생성한다. 즉 래핑되지 않은 값은 없다. 생성자는 가급적으로 쓰지 않는 것이 좋다. 
Array 생성자에는 특별한 형식이 하나 있는데, 인자로 숫자를 하나만 받으면, 그 숫자를 원소로 배열을 생성하는 것이 아니라 배열의 크기를 미리 정하는 기능이다. 이는 오래전 비 권장된 역사적 유물 탓. 
``` 
var a = new Array(1,2,3);
a; // [1,2,3]

var b = [1,2,3];
b;
``` 
# Date() and Error()
네이트브 생성자 Date()와 Error()는 리터럴 형식이 없으므로, 다른 네이트브에 비해 유용하다. date 객체 값은 new Date()로 생성한다. 이 생성자는 날짜/ 시각을 인자로 받는다. date 객체는 유닉스 타임스탬프 값을 얻는 영도로 가장 많이 쓰일 것이고, data 객체의 인스턴스로 부터 getTime()를 호출하면 된다.  하지만 es5에 정의된 정적 도우미 함수 Date.now()를 사용하는 게 더 쉽다. 폴리필로 쓴다면,
``` 
if(!Date.now){
    Date.now = function(){
        return (new Date()).getTime();
    };
}
``` 
``` 
typeof new Date(); // 'object'
typeof Date(); // 'string'
``` 
new 키워드 없이 Date()를 호출하면 현재 날짜, 시각에 해당하는 문자열을 반환한다. 

Error() 생송저눈 Array()와 마찬가지로 new가 있든 없든 결과는 똑같다. 

error 객체의 주 용도는 현재의 실행 스택 콘텍스트를 포착하여 객체에 담는 것이다. 이 실행 스택 콘텍스트는 함수 호출 스택, error 객체가 만들어진 줄 번호 등 디버깅에 도움이 될 만한 정보들을 담고 있다. 
error 객체는 보통 throw 연산자와 함깨 사용한다. 
```
function foo(x){
    if(!x){
        throw new Error('where is x!')
    }
}
```
Error 객체 인스턴스에는 적어도 message 프로퍼티가 들어있고, type 등 다른 프로퍼티가 포함될 때도 있다. 그러나 사람이 읽기 편한 포맷으로 에러 메시지를 보려면 방금전 언급한 stack  프로퍼티 대신, 그냥 error 객체의 toString()을 호출하는 것이 가장 좋다. 

# Symbol
심벌은 ES6에서 처음 선보인, 새로운 원시 값 타입이다. 심벌은 충돌 염려 없이 객체 프로퍼티로 사용 가능한, 특별한 '유일 값'이다. 주로 ES6의 특수한 내장 로직에 쓰기 위해 고안되었다. 

심벌은 프로퍼티 명으로도 사용할 수 있으나, 프로그램 코드나 개발자 콘솔 창에서 심벌의 실제 값을 보거나 접근하는 것은 불가능하다. 심벌 값을 콘솔 창에 출력해보면 Symbol(Symbol.create)로 나온다. 

ES6에는 심벌 몇 개가 미리 정의되어 있는데, Symbol.create, Symbol.iterator 식으로 Symbol 함수의 객체의 정적 프로퍼티로 접근한다. 
``` 
obj[Symbol.iterator] = function(){}
``` 
심벌을 직접 정의하려면, Symbol()네이티브를 사용한다. Symbol()은 앞에 new를 붙이면 에러가 나는 유일한 네이티브 생성자임. 기존에 전용/특수/내부 프로퍼티임으로, 주의를 주기 위해 습관적으로 썼던 _가 앞에 붙는 프로퍼티 명이 심볼로 대체될 수있다. 다시말하지만 심볼은 원시값이다. 객체 아님~! 

# 네이티브 프로토타입.
내장 네이티브 생성자는 각자의 .prototype 객체를 가진다. .prototype 객체에는 해당 객체의 하위 타입별로 고유한 로직이 담겨있다. 
```
String.prototype.indexOf() 문자열에서 특정 문자의 위치를 검색
String.prototype.charArt() 문자열에서 특정 위치의 문자를 반환
String.prototype.substr(), slice() , substring() 문자열의 일부를 새로운 문자열로 추출 
String.prototype.toUpperCase() , toLowerCase()  대문자/ 소문자로 변환된 새로운 문자열 생성
String.prototype.trim() 앞/뒤의 공란이 제거된 새로운 문자열 생성. 
```
위의 메소드는 문자열 값을 변경하지 않으며, 수정이 일어나면 기존값으로부터 새로운 값을 생성한다. 


참고: you don't know JS
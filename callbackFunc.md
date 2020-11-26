# 콜백함수란? 
콜백함수는 다른 코드의 인자를 넘겨주는 함수. 콜백 함수를 넘겨 받은 코드는 이 콜백 함수를 필요에 따라 적절한
시점에 실행하는 것이다. 

어떤 함수 x를 호출하면서, 특정 조건일 때 함수 y를 실행해서 나에게 알려달라는 요청을 함께 보낸다.
이 요청을 받은 함수 x 입장에서는 해당 조건이 갖춰졌는지 여부를 스스로 판단하고, y를 직접 호출한다. 

이처럼 콜백 함수는 다른 코드에서 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수이다. 콜백 함수를 위임받은 코드는 자체적인 내부 로직에 의해 이 콜백 함수를 적절한 시기에 실행한다.

```
var count = 0;
var timer = setInterval(function(){
    console.log(count);
    if(count++ > 4) clearInterval(timer);
}, 300);
```

첫 번째는 익명하수이고 두 번째는 300. setInterval.
# setInterval의 구조.
```
var intervalID = scope.setInterval(func,delay[,param,param2,...]);
```
scope에는 window 객체 또는 worker의 인스턴스가 들어올 수 있다. 두 객체 모두 setInterval 메서드를 제공. 일반적인 브라우저 환경에는 window 생략해서 함수처럼 사용 가능. 매개변수는 func, delau 값을 반드시 전달해야 한다. 세 번째 매개변수부터는 option. func은 함수이고, delay는 밀리초 단위의 슛자이고, 나머지 파라미터들은 func 함수를 실행할때 매개변수로 전달할 인자. func에 넘겨준 함수는 매 delay 마다 실행되며, 
! 그 결과 어떠한 값도 리턴하지 않음!
setInterval을 실행하면 반복적으로 실행되는 내용 자체를 특징할 수 있는 고유한 ID 값이 반환한다. 
이를 변수에 담는 이뉴는 반복 실행되는 중간에 종료할 수 있다. 

```
var count = 0;
var callbackFunc = function(){
    console.log(count);
    if(++count > 4) clearInterval(timer);
}
var timer = setInterval(callbackFunc,300)
//0
//1
//2
//3
//4
```
timer 변수에는 setInterval의 ID 값이 담김. setInterval에 전달한 첫 번째 인자인 callbackFunc 함수는 콜백함수. 0.3초마다 자동으로 실행된다. 콜백 함수 내부에서는 count 값을 출력하고, count를 1만큼 증가시키고, 그 값이 4 보다 크면 실행 종료! 

* setInterval이라고 하는 '다른 코드'에 첫번째 인자로서 callbackFunc 함수를 넘겨주자 제어권을 넘겨받은 setInterval이 스스로의 판단에 따라 적절한 시점에 익명함수를 실행. 이처럼 콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수 호출 시점에 대한 제어권을 가진다. 
즉! setInterval이 callbackFunc의 제어권을 넘겨 받음. 그리고 익명함수 실행. 

# 인자 
```
var callbackFunc = function(value,index){
  console.log(value,index);
  return value+ 'Adele';
}
var songList = ['hello','when we were young','rolling in the deep'].map(callbackFunc);

console.log(songList);
```
# Array.prototype.map(callback[, thisArg])
map 메서드는 첫 번째 인자로 callback 함수를 받고, 생략가능한 두 번째 인자로 콜백 함수 내부에서 this로 인식할 대상을 특정할 수 있음. 만약 thisArg를 생략할 경우에는 일반적인 함수와 마찬가지로 전역객체가 바인딩 된다. map 메서드는 메서드의 대상이 되는 배열의 모든 요소들을 처음부터 끝까지 하나씩 꺼내어 콜백함수를 반복 호출하고, 콜백함수의 실행 결과들을 모아 !새로운 배열을 만든다!. 콜백 함수의 첫번째 인자에는 배열의 요소중 현재 값, 두번째 인자에는 현재값의 인덱스, 세번째 인자에는 mpa 메서드의 대상이 되는 배열 자체가 담김. 
만약 변수이름을 바꾸더라도, 값은 이름에 따라 변하지 않음. 
콜백 함수를 호출하는 주체가 사용자가 아닌 map 메서드이므로 map 메서드가 콜백 함수를 호출할 때 인자에 어떤
값들을 어떤 순서로 넘길 것인지는 map 메서드에 따라 달라짐. 이처럼 콜백 함수를 호출할 때 인자에 어떤 값들을 넘길지에 대한 제어권을 가짐. 

# 콜백 지옥과 비동기 제어
콜백 지옥은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상. 자바스크립트에서 흔히 발생하는 문제이다. 주로 이벤터 처리나 서버 통신과 같이 비동기적인 작업을 수향하기 위해 이런 형태가 자주 등장한다. 가독성이 떨어지고, 코드를 수정하기 어렵다. 

동기적 코드는 현재 실행 중인 코드가 완료된 후에야 다음 코드를 실행하는 방식이다. cpu의 계산에 의해 즉시 처리가 가능한 대부분의 코드는 동기적인 코드. 계산식이 복잡해서 cpu가 계산하는 데 시간이 많이 필요한 경우라도 동기적 코드. 

비동기적 코드는 현재 실행 중인 코드의 완료 여부와 무관하게 즉시 다음 코드로 넘어간다.  사용자의 요청에 의해 특정 시간이 경과되기 전까지 어떤 함수의 실행을 보류한다거나(setimeout), 사용자의 직접적인 개입이 있을 때 비로소 어떤 함수를 실행하도록 대기한다거나(addEventListner), 웹브라우저 자체가 아닌 별도의 대상에 무언가를 요청하고 그에 대한 응답이 왔을때 비로소 어떤 함수를 실행하도록 대기하는(XMLHttpRequest), 별도의 요청, 실행 대기, 보류 등과 관련된 코드는 비동기적인 코드. 

# 콜백지옥 예시 
```
setTimeout(function(name){
  var coffeeList = name;
  console.log(coffeeList);

  setTimeout(function(name){
    coffeeList += ', ' + name;
    console.log(coffeeList);

    setTimeout(function(name){
      coffeeList += ', ' + name;
      console.log(coffeeList);
          
      setTimeout(function(name){
        coffeeList += ', ' + name;
        console.log(coffeeList);
      },500,'카페라떼');
    },500,'카페모카'); 
  },500,'아메리카노');
},500,'에스프레소');

```
# 콜백 지옥 해결 - 기명함수로 변환

```
var coffeeList = '';

var addEspresso = function(name){
  coffeeList = name;
  console.log(coffeeList);
  setTimeout(addAmericano,500,'아메라카노');
};
var addAmericano = function(name){
  coffeeList += ', '+ name;
  console.log(coffeeList);
  setTimeout(addMocha,500,'카페모카');
};
var addMocha = function(name){
  coffeeList += ', '+ name;
  console.log(coffeeList);
  setTimeout(addLatte,500,'카페라떼');
}

var addLatte = function(name){
  coffeeList += ', '+ name;
  console.log(coffeeList);
};
setTimeout(addEspreso,500,'에스프레소');

```
참고문헌: 코어 자바스크립트, 정재남 지음, 위키북스




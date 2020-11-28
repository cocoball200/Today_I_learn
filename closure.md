# 클로져 정의
A closure is the combination of a function and the lexical environment which that function was declared. 
클로저는 함수와 그 함수가 선언될 당시의 lexical environment의 상호관계에 따른 현상. 

어떤 컨텍스트 A에서 선언한 내부함수 B의 실행 컨텍스트가 활성화된 시점에서 B의 outerEvironmnetReference가 참조하는 대상인 A의 lexicalEnvironmnent에도 접근할수 있다. 즉, A에서는 B에서 선언한 변수에 접근할 수 없지만, B에서는 A에서 선언한 변수에 접근 가능하다. 

내부함수에서 외부 변수를 참조하는 경우에 한해서만 combination, 즉 '선언될 당시의 lexicalEnvironment와의 상호관계가 의미가 있다. 
어떤 함수에서 선언한 변수를 참조하는 내부 함수에서만 발생하는 현상. 

```
var outer = function(){
    var a = 1;
    var inner = function(){
        console.log(++a);
    };
    return inner();
};
outer();
```
outer 함수에서 변수 a를 선언하고, outer의 내부함수인 inner 함수에서 a의 값을 1만큼 증가시킨 다음 출력. inner 함수 내부에서는 a를 선언하지 않았기 때문에 environmnetRecord에서 값을 찾지 못하므로, ㅌenvironmnetRecord에 지정된 상위 컨텍스트인 outer의 lexicalEnvironmnent에 접근해서 다시 찾는다. 그래서 2가 출력된다. outer 함수의 실행 컨텍스트가 정리되면 lexicalEnvironmnet에 저장된 식별자들(a, inner)에 대한 참조를 지운다. 그러면 각 주소에 저장돼 있던 값들을 자신을 참조하는 변수가 하나도 없게 되므로 가비지 컬렉터의 수집대상이 된다. 

```
var outer =function(){
    var a = 1;
    var inner = function(){
        return ++a;
    };
    return inner;
};
var outer2 = outer(); //inner 함수자체 반환. 
console.log(outer2()); //2
console.log(outer2()); //3 
```
return inner() => 함수 실행 결과을 반환  과 return inner => inner 함수 자체를 반환. 
outer 함수의 실행 컨텍스트가 종료되기 이전에 inner 함수의 실행 컨텍스트가 종료돼 있으며, 이후 별도록 inner 함수를 호출할 수 없다.
return inner을 통해 outer컨텍스트가 종료된 후에도 inner함수를 호출할 수 있다. 왜냐하면 함수의 실행 컨텍스트가 종료될때 outer2 변수는 outer의 실행 결과인 inner 함수를 참조하게 된다. 그래서 outer2를 호출하면 앞에 반환된 함수인 inner가 실행된다. 
가비지 컬렉터는 어떤 값을 참조하는 변수가 하나라도 있으면 그 값은 수집 대상에 포함시키지 않는다. 
즉 outer의 실행이 종료되더라도, 내부 함수인 inner함수는 outer2를 실행할 가능성이 있기 때문이다.

# 클로저는 어떤 함수에서 선언한 변수를 참조하는 내부함수에서만 발생하는 현상이라고 한다. 
# 클로저란 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상. 

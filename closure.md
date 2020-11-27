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
    inner();
};
outer();
```
outer 함수에서 변수 a를 선언하고, outer의 내부함수인 inner 함수에서 a의 값을 1만큼 증가시킨 다음 출력. inner 함수 내부에서는 a를 선언하지 않았기 때문에 environmnetRecord에서 값을 찾지 못하므로, ㅌenvironmnetRecord에 지정된 상위 컨텍스트인 outer의 lexicalEnvironmnent에 접근해서 다시 찾는다. 그래서 2가 출력된다. outer 함수의 실행 컨텍스트가 정리되면 lexicalEnvironmnet에 저장된 식별자들(a, inner)에 대한 참조를 지운다. 그러면 각 주소에 저장돼 있던 값들을 자신을 참조하는 변수가 하나도 없게 되므로 가비지 컬렉터의 수집대상이 된다. 
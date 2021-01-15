# this ! 
this는 함수를 호출할 때 결정된다.
함수를 어떤 방식으로 호출하느냐에 따라 값이 달라진다.

# this in 전역공간
전역공간에서 this는 전역객체를 가리킨다. 전역컨테스트를 생성하는 주체는 전역 객체임으로. 

# 메서드로서 호출할때 this 와 함수로 호출할 때 this
함수를 실행하는 방법은 여러가지가 있지만, 일반적인 방법은 두가지는 함수로서 호출하는 것과 메서드로서 
호출하는 경우이다. 이 둘의 차이는 독립성이다. 함수는 그 자체로 독립적인 기능을 수행하고, 
메서드는 자신을 호출한 대상 객체에 관한 동작을 수행한다. 객체의 메서드로서 호출한 경우에만 메서드로 동작, 
그렇지 않으면 함수로 동작한다. 

```
var foo = function(x){
    console.log(this,x); // this는 window, 1
};
foo(1);

var obj = {
    method: foo
};
obj.method(2); // this는 obj/2  or 바꾸면 obj['method'](2); 

```
obj의 method 프로퍼티에 할당한 값과 func 변수에 할당한 값은 모두 15번째 줄에서 선언한 함수를 참조하지만, 
변수에 담아 호출한 경우와 obj의 객체의 프로퍼티에 할당해서 호출한 경우 this가 달라진다. 
간단하게 생각하면 함수앞에 점(.)이 있는지 여부로 파악.  foo(1)은 점이 없고, 함수로 호출, obj.method(2) method앞에 점이 있으니 메서드로 호출한 것이다. 

# 메서드 내부에서의 this
this에는 호출한 주체에 대한 정보가 담긴다. 어떤 함수를 메서드로 호출하는 경우 호출 주체는 바로 함수명 앞의 객체이다. 점 표기법의 경우 마지막 점 앞에서 명시된 객체가 곧 'this'이다. 
```
var obj = {
    methodA : function(){ console.log(this);},
    inner:{
        methodB: function(){console.log(this);}
    }
};
obj.methodA(); // === obj
obj['methodA'](); // === obj

obj.inner.methodB(); //  === obj.inner
obj.inner['methodB'](); // === obj.inner
obj['inner'].methodB(); //  === obj.inner
obj['inner']['methodB'](); // === obj.inner
```
# 함수 내부에서의 this
어떤 함수를 함수로서 호출할 경우에는 this가 지정되지 않는다. this에는 호출한 주체에 대한 정보가 담기지만, 함수로서 호출하는 것은 호출 주체를 명시하지 않고 개발자가 코드에 직접 관려하여 실행한 것이기 때문에, 호출의 주체의 정보를 알 수 없다. 실행 컨텍스트를 활성화할 당시 this가 지정되지 않는 경우, this는 전역객체를 본다. 
따라서 함수에서의 this는 전역객체를 가르킨다. 더글라스 크락포드는 이를 명백한 설계상의 오류로 지적. 

# 메서드 내부와 함수 내부의 this 예제. 

```
var obj1 = {
    outer: function(){
        console.log(this);  //obj1 
        var innerFunc = function(){
            console.log(this); // 전역객체window /obj2 
        }
        innerFunc();  // 함수로 호출 
        var obj2 = { 
        innerMethod:innerFunc
        };
        
        obj2.innerMethod(); //메서드로 호출 
    }
};
obj1.outer() //메서드로 호출 
```

# 메서드 내부 함수에서의 this를 우회하는 방법. 
위의 예제는 this에 대한 구분은 명확히 할 수 있지만, this라는 단어가 주는 인상과 조금 다르다. 
호출 주체가 없을 때는 자동으로 전역객체를 바인딩하지 않고, 호출 당시 주변환경의 this를 그대로 상속받고 싶다면,
훨씬 자연스럽고, 자바스크립트의 설계상 이런 식으로 동작하는 편이 스코프 체인과의 일관성을 지키는 설득력 있는 
방식일 수 있다. 변수를 검색하면 가장 가까운 스코프의 lexical environment 를 찾고 없으면 상위 스코프를 탐색하듯, this도 현재 컨텍스트에 바인딩된 대상이 없으면 직전 컨텍스트의 this를 바라보도록 한다면? 
그중 대표적인 방법은 바로 변수를 활용하는 것 

```
var obj = { outer: function(){
    console.log(this);
    var innerFunc1 = function(){
        console.log(this);
    }
    innerFunc();

    var self = this;
    var innerFunc2 = function(){
        console.log(self); //**이부분. 
    }
    innerFunc2();
    }
};
```
# this를 바인딩 하지 않는 함수 
this가 전역객체를 바라보는 문제를 보완하고자, this를 바인딩하지 않는 arrow function(화살표 함수)를 새로
도입했다. 화살표 함수는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지면서, 상위 스코프의 this를 그대로 활용할 수 있다. 

```
var obj = {
    outer: function(){
        console.log(this); // === obj
        var innerFunc = () => {
            console.log(this); // === obj
        };
        innerFunc(); 
    }
};
obj.outer();
```

# 콜백 함수 호출 시 그 함수 내부에서의 this 
함수 A 의 제어권을 다른 함수 또는 메서드를 B에게 넘겨주는 경우 함수 A를 콜백 함수라고 한다. 이때 함수 A는 함수 B의 내부 로직에 따라 실행되며, this도 함수 B 내부로직에서 정한 규칙에 따라 값이 결정된다. 콜백함수도 함수임으로 기본적으로 this가 전역객체를 참조하지만, 제어권을 받은 함수에서 콜백 함수에 별도로 this가 될 대상을 지정하는 경우 그 대상을 참조한다. 
setTime 함수와 forEach 메서드는 그 내부에서 콜백 함수를 호출할 때 대상이 될 this를 지정하지 않는다. 
그래서 콜백 함수 내부에서의 this는 전역객체를 참조한다. 
반면에 addEventListener 메서드는 콜백함수를 호출 할 때 자신의 this를 상속하도록 정의 되어있다.
곧 메서드의 점 앞부분이 this가 된다. 
특별하지 않은 경우는 기본적인 함수와 마찬가지로 전역객체를 바라본다. 

```
setTimeOut(function() {console.log(this);}, 300); //window
[1,2,3,4,5].forEach(function(x){
    console.log(this,x); // window
});

document.body.innerHTML += '<button id ="a">Click </button>';
document.body.querySelector('#a').addEventListener('click',function(e){
    console.log(this,e); // HTMLButtonElement
});
```

# 생성자 함수 내부에서 this
생성자 함수는 어떤 공통된 성질을 지니는 객체들을 생성하는 데 사용하는 함수이다. 객체지향 언어에서는 생성자를 클래스, 클래스를 통해 만든 객체를 인스턴스라고 한다. 프로그래밍적으로 생성자는 구체적인 인스턴스를 만들기 위한 일종의 틀이다. 이 틀에는 해당 클래스의 공통 속성들이 미리 준비돼어 있고, 여기에 인스턴스의 개성을 더해 개별 인스턴스를 만들 수 있다. 함수에 생성자로서 역할을 함께 부여하고, new 명령어와 함께 함수를 호출하면 해당 함수가 생성자로서 동작한다. 그리고 어떤 함수가 생성자 함수로서 호출된 경우,
! 내부에서의 this는 곧 새로 만들 구체적인 인스턴스 자신 !
생성자 함수는 함수명 첫번째 글자 대문자. 
 ```
 var Animal = function(name,sex){
     this.bark = 'walwal';
     this.name = name;
     this.sex = sex;
 };
 var happy = new Animal('happy','male');
 var roro = new Animal('roro','female')
 console.log(happy,roro);
 
 /*[object Object] {
  bark: "walwal",
  name: "happy",
  sex: "male"
}
[object Object] {
  bark: "walwal",
  name: "roro",
  sex: "female"
}*/
 ```
# 명시적으로 this를 바인딩 하는 방법. 
call 메서드는 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령이다. 이때 call 메서드의 첫 번째 인자를 this로 바인등하고, 이후 인자들을 호출한 함수의 매개변수로 한다. 함수를 그냥 실행하면, this는 전역객체를 참조하지만, call 메서드를 이용하면 임의의 객체를 this로 지정할 수 있다. 
```
var func = function(a,b,c){
    console.log(this,a,b,c);
};
func(1,2,3); //전역 
func.call({x:1},4,5,6); //this는 {x:1}

var obj = {
    a:1,
    method:function(x,y){
        console.log(this.a,x,y);
    }
};
obj.method(2,3) // 1,2,3
obj.method.call({a:4},5,6) // 4,5,6
```

apply 메서드는 call 메서드와 기능적으로 동일. call 메서드는 첫 번째 인자를 제외한 나머지 모든 인자들을 호출할 함수의 매개변수로 지정하는 반면, apply메서드는 두번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다는 점만 차이가 있다. 

```
var func = function(a,b,c){
    console.log(this,a,b,c);
};
func.apply({x:1},[4,5,6]); //this는 {x:1}

var obj = {
    a:1,
    method:function(x,y){
        console.log(this.a,x,y);
    }
};
obj.method.apply({a:4},[5,6]) // 4,5,6
```

# 유사배열객체에 배열 메서드를 적용.
객체에는 배열 메서드를 직접 적용할 수 없다. 하지만 키가 0 또는 ㅇ양의 정수인 프로퍼티가 존재하고,
length 프로퍼티의 값이 0 또는 양의 정수인 객체, 즉 배열의 구조와 유사한 객체의 경우 (유사배열객체) call or apply 메서드를 이용해 배열 메서드를 차용할 수 있습니다. arguments 객체, querySelectorAll, getElementsByClassName 등의 Node선택자로 선택한 결과인 NodeList도 마찬가지. 

```
var obj = { 
    0: 'a',
    1: 'b',
    2: 'c',
    length:3
};
var arr = Array.from(obj);
console.log(arr); // ['a','b','c']
```
# 전역 변수 
전역 변수를 선언하면 자바스크립트 엔진은 이를 전역객체의 프로퍼티로 할당한다. 
변수이면서, 객체의 프로퍼티이다. 자바스크립트의 모든 변수는 실은 특정 객체의 프로퍼티로 동작하기 때문이다. 

```
console.log(this)
```
참고문헌: 코어 자바스크립트, 정재남 지음, 위키북스
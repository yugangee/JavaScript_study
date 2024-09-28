# this
자바스크립트에서의 this는 어디서든 사용할 수 있다

## 상황에 따라 달라지는 this
자바스크립드의 this는 실행 컨텍스트가 생성될 때 함께 결정된다
즉, this는 함수를 호출할 때 결정된다

### 전역 공간에서의 this
전역 공간에서 this는 전역 객체  
브라우저 환경에서 전역객체는 window, Node.js 환경에서는  global  
전역 공간에서의 this  
-전역 공간에서의 this(브라우저 환경)  
```js
console.log(this); // { alert: f(), atob: f(), blur: f(), btoa: f(), ... }
console.log(window); // { alert: f(), atob: f(), blur: f(), btoa: f(), ... }
console.log(this === window); // true
```
-전역 공간에서의 this(Node.js 환경)  
```js
console.log(this); // { process: { title: 'node', version: 'v10.13.0',... } }
console.log(global); // { process: { title: 'node', version: 'v10.13.0',... } }
console.log(this === global); // true
```
-전역변수과 전역객체
```js
var a = 1; //전역 공간에서 선언한 변수 a에 1을 할당
console.log(a); // 1
console.log(window.a); // 1
console.log(this.a); // 1
```
전역공간에서의 this는 전역객체를 의미하므로 두 값이 같은 값을 출력하는 것은 당연하지만 그 값이 1인 것은 의아하다.  
그 이유는 자바스크립트의 모든 변수는 특정 객체의 프로퍼티로 동작하기 때문이다.  

-전역변수와 전역객체
```js
var a = 1; //전역변수로 선언한 경우
delete window.a; // false
console.log(a, window.a, this.a); // 1 1 1

var b = 2; //전역변수로 선언한 경우
delete b; // false
console.log(b, window.b, this.b); // 2 2 2

window.c = 3; //명시적으로 전역 객체 window에 프로퍼티를 추가한 것
delete window.c; // true
console.log(c, window.c, this.c); // Uncaught ReferenceError: c is not defined

window.d = 4; //명시적으로 전역 객체 window에 프로퍼티를 추가한 것
delete d; // true
console.log(d, window.d, this.d); // Uncaught ReferenceError: d is not defined
```
delete 연산자는 (window.)을 생략한 것  
처음부터 전역객체의 프로퍼티로 할당한 경우삭제가 된다  
전역변수로 선언한 경우에는 삭제가 되지 않는다  

### 메서드로서 호출할 때 그 메서드 내부에서의 this  

어떤 함수를 객체의 프로퍼티에 할당한다고 해서 그 자체로서 무조건 메서드가 되는 것이 아니라  
객체의 메서드로서 호출할 경우에만 메서드로 작동하고 그렇지 않으면 함수로 동작한다.  

-함수로서 호출, 메서드로서 호출
```js
var func = function(x) { //func변수에 익명함수 할당
  console.log(this, x);
};
func(1); // Window { ... } 1 //func호출: this로 전역객체 window 출력

var obj = { //obj라는 변수에 객체를 할당. 그 객체의 method 프로퍼티 앞에서 만든 func 함수 할당
  method: func,
};
obj.method(2); // { method: f } 2//obj의 methos호출: this가 obj
```
obj의 method프로퍼티에 할당한 값과 func변수에 할당한 값은 모두 1번째 줄에서 선언한 함수를 참조한다.

### 함수로서의 호출과 메서드로서 호출  
앞에 점이 없으면 함수로서 호출  
앞에 점이 있으면 메서드로서 호출

-메서드로서 호출
```js
var obj = {
  method: function(x) {
    console.log(this, x);
  },
};
obj.method(1); // { method: f } 1
obj['method'](2); // { method: f } 2
```

### 함수로서 호출할 때 그 함수 내부에서의 this
함수를 함수로서 호출할 경우에는 this가 지정되지 않는다.  
-내부함수에서의 this  
```js
var obj1 = { //객체생성
  outer: function() { //obj1.outer함수의 실행 컨텍스트가 생성괴면서 호이스팅, 스코프 체인 정보를 수집하고 this 바인딩. this에는 마지막 점 앞의 객체인 obj1이 바인딩됨.
    console.log(this); // (1)obj1
    var innerFunc = function() `  //호이스팅된 변수 innerFunc는 outer 스코프 내에서만 접근할 수 있는 지역변수
      console.log(this); // (2)innerFunc호출 결과: 전역객체(Window) (3)obj2.innerMethod호출: obj2.innerMethod
    };
    innerFunc(); 

    var obj2 = { //obj2역시 outer스코프 내에서만 접근할 수 있는 지역변수
      innerMethod: innerFunc,
    };
    obj2.innerMethod();
  },
};
obj1.outer(); //obj1.outer 호출
```  

### 생성자 함수 내부에서의 this


## 명시적으로 this를 바인딩하는 방법
### call 메서드

### apply 메서드

### call/apply 메서드

### bind 메서드

### 화살표 함수의 예외사항

### 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)





```js

```

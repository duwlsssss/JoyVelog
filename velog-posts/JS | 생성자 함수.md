<p><strong>생성자 함수</strong>constructor: <code>new 키워드</code>와 함께 호출돼 객체(인스턴스)를 생성하는 함수</p>
<p>함수 선언문 or 함수 표현식으로 정의한 함수는 <code>일반 함수</code>로써 호출되는 것 외에도 <code>new 키워드</code>와 함께 <code>호출</code>돼<code>객체를 생성</code>하는 <code>생성자 함수</code>로 호출될 수 있음</p>
<p><strong>프로퍼티 구조가 동일한 인스턴스</strong>를 생성하기(+초기화) 위한 템플릿으로 동작함</p>
<pre><code class="language-jsx">// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius; // this에 프로퍼티를 추가하고, 프로퍼티의 초기값으로 전달된 인자를 할당 
  this.getDiameter = function () { 
    return 2 * this.radius;
  };
}

// 인스턴스 생성 -&gt; js 엔진이 암묵적으로 인스턴스를 생성하고, 초기화하고 반환함 
const circle1 = new Circle(5);</code></pre>
<h3 id="✅-생성자-함수의-인스턴스-생성-과정">✅ 생성자 함수의 인스턴스 생성 과정</h3>
<p><strong>1. 암묵적으로 빈 객체(인스턴스)가 생성되고, 생성자 함수 내부의 this에 바인딩됨</strong>
+바인딩은 식별자와 값을 연결하는 과정, this 바인딩은 this(키워드이지만 식별자 역할을 함), this를 가리킬 객체를 연결하는 것 / js에선 런타임에서 함수 호출 방식에 따라 동적으로 this 바인딩이 결정됨</p>
<p><del>딥다이브 책에선 이 과정이 런타임 전에 실행된다는데, 함수가 일반 함수, 생성자 함수 중 뭘로 호출될지 모르는 상황에서 this를 어떻게 결정한다는 건진 모르겠다...
this 값은 함수가 어떻게 호출되었는지에 따라 다르게 결정되니까 함수의 정의 시점이 아닌 호출 시점에 결정돼야하는 게 맞지 않나?</del></p>
<p><strong>2. 코드가 실행되며 this에 바인딩된 인스턴스가 초기화됨</strong>
<strong>3. 완성된 인스턴스가 바인딩된 this가 반환됨</strong>
명시적으로 다른 객체를 반환하면 this가 반환되지 않고 return 문에 명시한 객체가 반환됨 or 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 this가 반환됨 -&gt; 생성자 함수 내부에선 return 문 반드시 생략해야함(매서드 return 말하는 건 아님)</p>
<pre><code class="language-jsx">function Circle(radius) {
  // 1. 암묵적으로 인스턴스를 생성하고 this에 바인딩

  // 2. this에 바인딩돼 있는 인스턴스를 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환
}

// 인스턴스 생성
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}</code></pre>
<pre><code class="language-jsx">// ➕ new 없이 일반 함수로 호출하면 
function Circle(radius) {
  this.radius = radius; // this는 전역 객체를 참조
  // strict mode에서는 전역 객체가 바인딩 대상에서 제외돼, 바인딩 될 객체가 없는 this는 undefined 값을 가짐
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = Circle(1); // new 없이 일반 함수로 호출

console.log(circle1); // undefined    반환문이 없으니까 암묵적으로 undefined 반환
// 전역 객체의 프로퍼티로 추가된 radius와 getDiameter()
console.log(radius);    // 1    
console.log(getDiameter()); // 2    

circle1.getDiameter() // TypeError: Cannot read property 'getDiameter' of undefined</code></pre>
<h3 id="✅-내부-메서드-call과-construct">✅ 내부 메서드 [[Call]]과 [[Construct]]</h3>
<p>함수는 일급객체 -&gt; callable -&gt; [[Call]] 내부 메서드를 추가로 가짐 </p>
<p>모든 함수객체는 callable 이지만 <code>함수 정의 방식</code>에 따라 <code>constructor</code>일 수도 있고 <code>non-constructor</code>일 수도 있음</p>
<p>함수객체를 일반 함수로써 호출하면 [[Call]] 내부 메서드가, 생성자 함수로써 호출하면 [[Constuct]] 내부 메서드가 호출됨. 이때 non-constructor라 [[Constuct]] 내부 메서드를 갖지 않으면 typeError 발생</p>
<ul>
<li>constructor: 함수 선언문, 함수 표현식, 클래스
constructor만 prototype 프로퍼티를 가짐<pre><code class="language-jsx">console.log(function () {}.hasOwnProperty(&quot;prototype&quot;)); // true    함수 객체는 prototype 프로퍼티를 소유
console.log({}.hasOwnProperty(&quot;prototype&quot;)); // false    일반 객체는 prototype 프로퍼티를 소유하지 X</code></pre>
</li>
<li>non-constructor: 메서드(ES6의 메서드 축약표현), 화살표함수<pre><code class="language-jsx">// ✔ constructor
// 함수 선언문
function foo() {}
const baz = {
x: function () {}
};
// 함수 표현식
const bar = function () {};
// 생성자 함수에서는 메서드 축약 표현을 사용할 수 없고, 함수 표현식을 통해 메서드를 정의함 -&gt; 생성자 함수 안에서 정의된 함수는 기본적으로 constructor임
function Circle(radius) {
this.radius = radius; 
// getDiameter는 메서드라 부르지만, 엄밀히 말하면 함수 표현식으로 정의했기 때문에 constructor임
this.getDiameter = function () { 
  return 2 * this.radius;
};
}
</code></pre>
</li>
</ul>
<p>// 일반 함수로 정의된 함수만이 constructor
// 반환값이 없거나 원시값이어서 new 키워드로 생성되고 this에 바인딩된 빈 객체를 반환함 
new foo();   // {}<br />new bar();   // {}
new baz.x(); // {}
new circle1.getDiameter(); // {}</p>
<p>// ✔ non-constructor
// 화살표 함수 정의
const arrow = () =&gt; {};
// ES6의 메서드 축약 표현만을 메서드로 인정함
const obj = {
  x() {}
};</p>
<p>// non-constructor는 [[Constuct]]가 없어 TypeError 발생 
new arrow(); // TypeError: arrow is not a constructor
new obj.x(); // TypeError: obj.x is not a constructor</p>
<pre><code>
### ✅ new.target

생성자 함수가 new 키워드 없이 호출됐을때 일반함수가 아닌 생성자함수로 처리하기 위해 new.target 사용(ES6)

생성자 함수로 호출된 함수 내부의 new.target은 함수 자신을 가리킴
일반 함수로 호출된 함수 내부의 new.target은 undefined

```jsx
// 생성자 함수
function Circle(radius) {
  if (!new.target) { // 이 함수가 일반함수로 호출됐을 때
    return new Circle(radius); // new 연산자와 함께 생성자 함수를 재귀 호출해 생성된 인스턴스를 반환
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출해도 재귀호출을 통해 생성자 함수로 호출됨
const circle = Circle(5);
console.log(circle.getDiameter());  // 10</code></pre><p>대부분의 빌트인 생성자 함수(Object, String, Number, Function, Array, Date ...)는 생성자 함수로 호출됐는지 확인 후 적절한 값을 반환함 </p>
<pre><code class="language-jsx">const obj = new Object();
console.log(obj); // {}
const obj_ = Object();
console.log(obj_); // {}

const str_object = new String(123);
console.log(str_object, typeof str_object); // [String: '123'] object
const str = String(123);
console.log(str, typeof str); // 123 string</code></pre>


<blockquote>
<p>참고자료
<a href="https://velog.io/@kimlj0814/JS-%EB%B3%80%EC%88%98-%EC%83%9D%EC%84%B1-%EA%B3%BC%EC%A0%95-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85#:~:text=%EC%B0%B8%EA%B3%A0%EC%9E%90%EB%A3%8C-,%EB%AA%A8%EB%8D%98%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%20Deep%20Dive,-%ED%81%B4%EB%A6%B0%EC%BD%94%EB%93%9C%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8">모던 자바스크립트 Deep Dive</a>
<a href="https://www.udemy.com/course/clean-code-js/?couponCode=OF83024D">클린코드 자바스크립트</a></p>
</blockquote>
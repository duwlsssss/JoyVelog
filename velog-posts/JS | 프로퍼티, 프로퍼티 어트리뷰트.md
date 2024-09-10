<p>+내부 슬롯과 내부 메서드: [[...]], ECMAScript 사양에 정의된 대로 구현된 js의 내부로직으로 JS 엔진에서 동작, 개발자가 직접 접근은 불가, 일부는 간접적으로 접근할 수 있는 수단 제공함_js엔진이 관리하는 내부 상태값으로 볼 수 있음 </p>
<h3 id="✅-프로퍼티">✅ 프로퍼티</h3>
<p>🟡 <strong>데이터 프로퍼티</strong>data property: key-value로 구성된 일반적인 프로퍼티</p>
<p><strong>프로퍼티를 생성할때</strong> js엔진에 의해 <code>프로퍼티 어트리뷰트</code>(프로퍼티 상태)가 기본값으로 자동 정의됨 </p>
<p>👇 데이터 프로퍼티의 프로퍼티 어트리뷰트를 나타낸 표</p>
<table>
<thead>
<tr>
<th>프로퍼티 어트리뷰트</th>
<th>프로퍼티 디스크립터 객체의 프로퍼티(키)</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>[[Value]]</td>
<td>value</td>
<td>프로퍼티가 담고 있는 실제 값 (default: 프로퍼티의 값)</td>
</tr>
<tr>
<td>[[Writable]]</td>
<td>writable</td>
<td>프로퍼티 값 변경 가능 여부 boolean (default: true)</td>
</tr>
<tr>
<td>[[Enumerable]]</td>
<td>enumerable</td>
<td>프로퍼티의 열거 가능 여부 boolean (default: true)</td>
</tr>
<tr>
<td>[[configurable]]</td>
<td>configurable</td>
<td>프로퍼티 어트리뷰트 재정의 가능 여부 boolean (default: true)</td>
</tr>
</tbody></table>
<p>+[[writable]]이 false면 해당 프로퍼티의 [[value]] 값을 변경할 수 없는 <strong>읽기 전용 프로퍼티</strong>가 됨 / [[configurable]]이 false면 해당 프로퍼티 삭제, 다른 속성들은 변경할 수 없음 but [[writable]] 이 true면 [[Value]]값 수정 가능, 단 [[writable]]을 한 번 false로 바꾼 다음음 모든 속성 못 바꿈_프로퍼티 구조를 고정하면서도 값은 필요할 때까지 변경 가능한 유연성 제공 </p>
<p>+[[Writable]].[[Enumerable]],[[configurable]]의 기본값을 true라 했는데, 이건 <strong>객체 리터럴로 객체를 정의하는 경우</strong>나 <strong>동적으로 프로퍼티를 추가하는 경우</strong>만임, Object.defineProperty()를 사용해 프로퍼티를 정의하면 기본값이 false임</p>
<pre><code class="language-jsx">const person = {
  name: &quot;WI&quot;,
};

// age 프로퍼티 동적 생성도 마찬가지로 초기화됨 
person.age = 100;

// ✅ 프로퍼티 어트리뷰트는 내부슬롯이라 메서드를 통해 간접적으로 확인 가능
// '한 프로퍼티의 프로퍼티 어트리뷰트 정보'를 제공하는 '프로퍼티 디스크립터 객체'를 반환
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: &quot;WI&quot;, writable: true, enumerable: true, configurable: true}

// (ES8) '모든 프로퍼티의 프로퍼티 어트리뷰트 정보'를 제공하는 프로퍼티 디스크립터 객체들을 반환 
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: { value: 'WI', writable: true, enumerable: true, configurable: true },
  age: { value: 100, writable: true, enumerable: true, configurable: true }
}
 */</code></pre>
<p>🟡 <strong>접근자 프로퍼티</strong> accessor property: 자체적으로 값을 갖지 않고, 데이터 프로퍼티의 값을 읽거나, 저장할 때 호출되는 <code>접근자 함수</code>access function 로 구성된 프로퍼티 </p>
<table>
<thead>
<tr>
<th>프로퍼티 어트리뷰트</th>
<th>프로퍼티 디스크립터 객체의 프로퍼티(키)</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>[[Get]]</td>
<td>get</td>
<td>접근자 프로퍼티 키로 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수-getter() 호출 (default: undefined)</td>
</tr>
<tr>
<td>[[Set]]</td>
<td>set</td>
<td>접근자 프로퍼티 키로 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수-setter() 호출 (default: undefined)</td>
</tr>
<tr>
<td>[[Enumerable]]</td>
<td>enumerable</td>
<td>프로퍼티의 열거 가능 여부 boolean (default: true)</td>
</tr>
<tr>
<td>[[configurable]]</td>
<td>configurable</td>
<td>프로퍼티 어트리뷰트 재정의 가능 여부 boolean (default: true)</td>
</tr>
</tbody></table>
<p>접근자 함수는 getter / setter 함수 라고도 함, 모두 정의하거나 하나만 정의할 수도 있고, 아예 정의 안할 수도 있음</p>
<pre><code class="language-jsx">const person = {
  firstName: &quot;dodam&quot;,
  lastName: &quot;kim&quot;,

  // 접근자 함수로 구성된 접근자 프로퍼티
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(&quot; &quot;);
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + &quot; &quot; + person.lastName); // dodam kim

// ✔ 접근자 프로퍼티를 통한 프로퍼티 값의 참조 ([[Get]]의 값인 getter 함수가 호출돼 그 결과가 반환됨)
console.log(person.fullName); // dodam kim

// ✔ 접근자 프로퍼티를 통한 프로퍼티 값의 저장 ([[Set]]의 값인 setter 함수가 호출돼 그 결과가 저장됨 )
person.fullName = &quot;dodame kim&quot;;
console.log(person); // { firstName: 'dodame', lastName: 'kim', fullName: [Getter/Setter] }


// firstName 프로퍼티는 '데이터 프로퍼티'
console.log(Object.getOwnPropertyDescriptor(person, &quot;firstName&quot;));
/*
{
  value: 'Youngmaaan',
  writable: true,
  enumerable: true,
  configurable: true
} 
*/

// &quot;fullName&quot; 프로퍼티는 '접근자 프로퍼티'
console.log(Object.getOwnPropertyDescriptor(person, &quot;fullName&quot;));
/*
{
  get: [Function: get fullName],
  set: [Function: set fullName],
  enumerable: true,
  configurable: true
}
*/</code></pre>


<h3 id="✅-프로퍼티-정의">✅ 프로퍼티 정의</h3>
<p>: 새로운 프로퍼티를 추가하며 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것</p>
<pre><code class="language-jsx">const person = {};

// 데이터 프로퍼티 정의
Object.defineProperties(person, {
  // 데이터 프로퍼티
  firstName: {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true
  },
  lastName: {
  value: 'Lee'
  },
  //접근자 프로퍼티
  fullName: {
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  },
});

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  firstName: {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Lee',
    writable: false, // 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
    enumerable: false,
    configurable: false
  },
  fullName: {
    get: [Function: get],
    set: [Function: set],
    enumerable: true,
    configurable: true
  }
}
*/

// ✔ [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경 불가
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없음
// ❕ 이때 값을 변경하면 에러 발생x, 무시됨 
person.lastName = 'Kim';

// ✔ [[Enumerable]]의 값이 false인 경우
// 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거 불가
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않음
console.log(Object.keys(person)); // [ 'firstName', 'fullName' ]

// ✔ [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제 불가
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없음
// ❕ 이때 값을 프로퍼티를 삭제하면 에러 발생x, 무시됨 
delete person.lastName;

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티 어트리뷰트를 재정의 불가
Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName</code></pre>
<p>각 프로퍼티에 대해 모든 프로퍼티 어트리뷰트를 설정할 필요는 없음, 설정하지 않을 경우 default 로 설정되는 값들이 있음</p>
<p>||프로퍼티 디스크립터 객체의 프로퍼티|대응하는 프로퍼티 어트리뷰트|생략했을 경우 Default value|
|:|:|:|
|value|[[Value]]|undefined|
|get|[[GET]]|undefined|
|set|[[SET]]|undefined|
|writable|[[Writable]]|false|
|enumerable|[[Enumerable]]|false|
|configurable|[[configurable]]|false|</p>


<h3 id="✅-객체-변경-방지">✅ 객체 변경 방지</h3>
<p>객체는 변경가능한 값. 프로퍼티 추가, 프로퍼티 삭제, 프로퍼티 값 갱신, 프로퍼티 어트리뷰트 재정의도 할 수 있음
➡ 객체가 변경을 방지 메서드 사용, 각 메서드마다 제한하는 강도가 다름 </p>
<p>|구분|메서드|확인 메서드|프로퍼티 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기(갱신)|프로퍼티 어트리뷰트 재정의|
|:|:|:|:|:|:|:|
|<strong>객체 확장 금지</strong>-프로퍼티 추가 불가|Object.preventExtensions|Object.isExtensible|X|O|O|O|O|
|<strong>객체 밀봉</strong>-읽기,쓰기만 가능|Object.seal|Object.isSealede|X|X|O|O|X|
|<strong>객체 동결</strong>-읽기만 가능|Object.freeze|Object.isFrozen|X|X|O|X|X|</p>
<pre><code class="language-jsx">const person = { name: 'Lee' };

// person 객체를 동결(freeze)
Object.freeze(person);

// person 객체는 동결(freeze)된 객체
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false
console.log(Object.getOwnPropertyDescriptors(person));
/*
  {
    name: {value: &quot;Lee&quot;, writable: false, enumerable: true, configurable: false},
  }
  */

// 프로퍼티 추가가 금지됨
person.age = 20; // 무시. strict mode에서는 TypeError: Cannot add property age, object is not extensible
console.log(person); // {name: &quot;Lee&quot;}

// 프로퍼티 삭제가 금지됨
delete person.name; // 무시. strict mode에서는 TypeError: Cannot delete property 'name' of #&lt;Object&gt;
console.log(person); // {name: &quot;Lee&quot;}

// 프로퍼티 값 갱신이 금지됨
person.name = 'Kim'; // 무시. strict mode에서는 TypeError: Cannot assign to read only property 'name' of object '#&lt;Object&gt;'
console.log(person); // {name: &quot;Lee&quot;}

// 프로퍼티 어트리뷰트 재정의가 금지됨
Object.defineProperty(person, 'name', { configurable: true }); // TypeError: Cannot redefine property: name</code></pre>
<p>strict mode에서는 허용하지 않는 프로퍼티 제어 동작에 대해선 에러 발생시킴, strict mode가 아닌 경우 해당 명령 무시함</p>
<p>객체 변경 방지 메서드들은 <code>얕은</code>shallow <code>변경 방지</code>로 직속 프로퍼티만 동결됨, <strong>중첩 객체는 참조값으로 저장돼 동결 안됨</strong> ➡ 중첩 객체까지 동결하려면 재귀 호출을 통해 객체의 모든 요소에 Object.freeze를 적용해야함</p>
<pre><code class="language-jsx">const person = {
  name: &quot;WI&quot;,
  age: 100,
  address: {
    city: &quot;Incheon&quot;,
  },
};


// 💩 중첩 객체가 있는 상황에서 얕은 객체 변경 방지(Shallow)
Object.freeze(person);
console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // false    중첩된 address 프로퍼티에 대해서는 freeze 되지 않음

person.address.city = &quot;Seoul&quot;;
console.log(person); // { name: 'WI', age: 100, address: { city: 'Seoul' } }    프로퍼티 값이 갱신됐음</code></pre>
<pre><code class="language-jsx">const person = {
  name: &quot;WI&quot;,
  age: 100,
  address: {
    city: &quot;Incheon&quot;,
  },
};

// 👍 깊은 객체 변경 방지(Deep)
// 입력 객체의 깊은 복사본을 생성해 반환하는 함수 
function deepFreeze(obj) {

   if (obj &amp;&amp; typeof obj === &quot;object&quot; &amp;&amp; !Object.isFrozen(obj)) {
    // 일단 현재 들어온 객체를 동결시키고
    Object.freeze(obj);
    // 다른 프로퍼티 중, 중첩 객체들을 고려해 deepFreeze 함수 재귀실행
    Object.keys(obj).forEach((key) =&gt; deepFreeze(obj[key]));
  }
}

// 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // true    깊은 객체 변경 방지로 중접된 객체에 대해서도 동결됨

person.address.city = &quot;Seoul&quot;;
console.log(person); // { name: 'WI', age: 100, address: { city: 'Incheon' } }    중접 객체의 프로퍼티 값도 갱신되지 않음</code></pre>


<p>+<strong>strict mode</strong>: js의 문법을 더 <code>엄격히</code> 적용해 오류를 발생시킬 가능성이 높거나, js 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 <strong>명시적 에러를 발생시킴</strong> </p>
<ul>
<li><p>strict mode 적용 방법:
즉시 실행 함수로 감싼 스크립트 단위 선두에 'use strict'를 써서 strict mode 적용</p>
<pre><code class="language-jsx">// 즉시 실행함수 내부에서 use strict 적용
(function () {
'use strict';
...
})();</code></pre>
</li>
<li><p><em>ESLint같은 Lint 도구는 소스코드 실행 전 코드를 스캔해 strict mode가 제한하는 오류를 리포팅,코딩 컨벤션을 파일형태로 설정하고 강제할 수 있어 린트 도구 쓰는게 나음*</em></p>
</li>
<li><p>strict mode가 발생시키는 에러: </p>
<pre><code class="language-jsx">// ✅ 선언하지 않은 변수를 참조하면 referenceError 발생 
x = 1;
console.log(x); // ReferenceError: x is not defined
</code></pre>
</li>
</ul>
<p>// ✅ 변수,함수,매개변수를 delete 연산자로 삭제하려하면 syntaxError 발생
var x = 1;
delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.</p>
<p>function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
}</p>
<p>delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.</p>
<p>// ✅ 중복된 매개변수 이름을 사용하면 SyntaxError 발생 
function foo(x, x) { //SyntaxError: Duplicate parameter name not allowed in this context
    return x + x;
  }
  console.log(foo(1, 2));</p>
<pre><code>strict mode에서 함수를 **일반 함수로 호출하면 this에 undefined가 바인딩됨**_생성자 함수가 아닌 일반 함수 내부에선 this 를 사용할 필요가 없기 때문
```jsx
function foo() {
  console.log(this); // undefined
}
foo(); // 오류는 발생x

function Foo() {
  console.log(this); // Foo
}
new Foo();

//non-strict 모드에선 일반함수로 호출된 함수의 this가 전역 객체를 참조함 
function foo() {
  console.log(this); // 전역 객체 (브라우저에서는 window, Node.js에서는 global)
}
foo();</code></pre><blockquote>
<p>참고자료
<a href="https://velog.io/@kimlj0814/JS-%EB%B3%80%EC%88%98-%EC%83%9D%EC%84%B1-%EA%B3%BC%EC%A0%95-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85#:~:text=%EC%B0%B8%EA%B3%A0%EC%9E%90%EB%A3%8C-,%EB%AA%A8%EB%8D%98%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%20Deep%20Dive,-%ED%81%B4%EB%A6%B0%EC%BD%94%EB%93%9C%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8">모던 자바스크립트 Deep Dive</a>
<a href="https://www.udemy.com/course/clean-code-js/?couponCode=OF83024D">클린코드 자바스크립트</a></p>
</blockquote>
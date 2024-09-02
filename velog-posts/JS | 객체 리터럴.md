<h3 id="✅-객체">✅ 객체</h3>
<p>js는 <code>객체</code>Object 기반의 프로그래밍 언어이고, js를 구성하는 거의 모든 것이 객체임_함수,배열,정규표현식 등 </p>
<p>primitive type은 하나의 값만 나타냄 / reference type은 다양한 타입의 값을 <code>하나의 단위로 구성</code>한 <code>복합적인 자료구조</code>임</p>
<p>객체는 key, value 쌍의 <code>프로퍼티</code>로 구성됨
<img src="https://velog.velcdn.com/images/kimlj0814/post/17beef5b-1868-4680-837d-d2845aecf607/image.png" width="60%" /></p>
<p>c++, java는 <code>클래스 기반 객체지향 언어</code>(Object Oriented Programming, OOP)로 클래스를 사전에 정의하고, 필요한 시점에 new 연산자로 생성자를 호출해 인스턴스를 생성하는 방식으로 객체 생성함
javascript는 <code>prototype 기반 객체 지향 언어</code>로써 <strong>다양한 객체 생성 방법</strong> 지원함</p>
<ul>
<li>객체 리터럴</li>
<li>Object 생성자 함수</li>
<li>생성자 함수</li>
<li>Object.create 메서드</li>
<li>클래스 (ES6)</li>
</ul>
<p>이 중 가장 일반적인 방법은 객체 리터럴을 통한 방법임</p>
<h3 id="✅-객체-리터럴">✅ 객체 리터럴</h3>
<p>중괄호<code>{...}</code>내에 0개 이상의 프로퍼티를 정의, 객체 리터럴이 <code>변수에 할당되는 시점</code>에 js엔진이 평가해 객체를 생성함 
객체 리터럴의 중괄호는 <strong>코드 블록을 의미하는 게 아님</strong>, 객체 리터럴은 <strong>값으로 평가되는 표현식</strong>임 </p>
<ul>
<li>프로퍼티 키: 문자열 or 심볼값</li>
<li>프로퍼티 값: 모든 값</li>
</ul>
<pre><code class="language-jsx">const arrayEX ={
  firstName:'Kim', 
  'last-name':'dodam', // 식별자 네이밍 규칙을 따르지 않는 프로퍼티 키는 따옴표로 감싸야 함, 안그럼 SyntaxError 발생 -&gt; 식별자 네이밍 규칙 지키는 게 좋음
  '':'', // 빈 문자열도 프로퍼티 키로 사용 가능 -&gt; 키로써 의미를 갖지 않으므로 사용 지양하자 
  1:1, // 프로퍼티 키에 문자열,심볼값 외 값을 사용하면 타입 캐스팅으로 문자열로 해석
  1:2, // 프로퍼티를 중복 선언하면 나중에 선언한 프로퍼티가 덮어씀, 에러 발생x 
  // ✔ 프로퍼티 값으로 익명 함수를 할당할 수 있음
  // ES6이전에선 이것을 메서드라 함 
  // 프로퍼티는 객체의 상태를 나타냄data, 메서드는 프로퍼티를 참조하고 조작할 수 있는 동작임behavior
  sayHello: function(){
    console.log('Hello, I am '+this.firstName)
  },
  // ⭐ ES6부턴 메서드 축약 표현으로 정의된 함수만을 메서드라 함, 메서드: 객체에 묶여 있는 함수 
  sayHello_(){
    console.log('Hello, I am '+this.firstName)
  }, 
};

/** ✔ 프로퍼티에 접근하는 방법: 프로퍼티 키가 식별자 네이밍 규칙을 준수하면 
  * 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법,
  * 대괄호 프로퍼티 접근 연산자([])를 사용하는 대괄호 표기법으로 접근 가능
  */
console.log(typeof arrayEX); // &quot;object&quot;
console.log(arrayEX.firstName); // &quot;Kim&quot;
console.log(arrayEX['firstName']); // 대괄호 표기법 사용하면 프로퍼티 키를 따옴표로 감싸줘야 함. 안그럼 js 엔진이 식별자로 해석함. 없으니까 referenceError 발생
console.log(arrayEX.name) // 객체에 존재하지 않는 프로퍼티에 접근하면 에러가 아닌, undefined 발생
// ㄴ 객체가 존재하는 경우 js는 단순히 프로퍼티가 없는 것으로 간주하고 undefined를 반환, 객체 자체가 존재하지 않을 땐 ReferenceError 발생
console.log(arrayEX['last-name']); // &quot;dodam&quot;
console.log(arrayEX['']); // '' 
console.log(arrayEX['1']); // 2    arrayEX[1]로 써도 됨
arrayEX.sayHello(); // &quot;Hello, I am Kim&quot;
console.log(arrayEX.sayHello); // [Function: sayHello]    함수 객체에 대한 정보를 출력     </code></pre>

◽ 프로퍼티 값 갱신, 생성, 삭제 

<pre><code class="language-jsx">const puppy = {
  name: 'dodam'
};

// ✔ 프로퍼티 값 갱신
// 이미 존재하는 프로퍼티에 값 할당
puppy.name='bamtol';

// ✔ 프로퍼티 동적 생성
// 존재하지 않는 프로퍼티에 값 할당하면 프로퍼티가 동적으로 생성되고, 값 할당됨 
puppy.age=7;

console.log(puppy); // { name: 'bamtol', age: 7 }

// ✔ 프로퍼티 삭제
// delete 연산자를 활용
delete puppy.age;
delete puppy.birthDay; // 없는 프로퍼티를 삭제해도 에러 안 뜸 

console.log(puppy); // { name: 'bamtol' }</code></pre>

◽ ES6에서 확장된 객체 리터럴의 기능

<p>✔ 프로퍼티 축약 표현
프로퍼티 값으로 변수를 사용할 떄 <strong><code>변수이름</code>과 <code>프로퍼티 키</code>가 같으면 프로퍼티 키 생략 가능</strong></p>
<pre><code class="language-jsx">let x=1,y=2;
const value = {x,y};

console.log(value); // { x: 1, y: 2 }</code></pre>
<p>✔ 객체 리터럴 내부에서 <strong>개선된 프로퍼티 이름 사용</strong>
computed property name: <code>문자열로 평가될 수 있는 표현식</code>을 사용해 <strong>프로퍼티 키를 동적으로 생성</strong>, 표현식을 []로 감싸야 함</p>
<pre><code class="language-jsx">const prefix = 'prop';
let i =0;
const obj = {
  [`${prefix}+${++i}`]:i,
  [`${prefix}+${++i}`]:i,
};
console.log(obj); // { &quot;prefix+1&quot;: 1, &quot;prefix+2&quot;: 2 }</code></pre>
<p>✔ 메서드 축약 표현 
객체 리터럴 안에서 메서드를 정의할 때 <code>function 키워드를 생략한 축약 표현</code> 사용 가능 </p>
<pre><code class="language-jsx">const methodTest={
  name:'kim',
  sayHi() {
    console.log('Hi, I am '+this.name)
  }
};

methodTest.sayHi(); // Hi, I am kim</code></pre>
<p>➕ <strong>메서드</strong>와 ES5의 프로퍼티의 값으로 할당된 <strong>일반 함수</strong>의 차이점</p>
<table>
<thead>
<tr>
<th>일반 함수</th>
<th>메서드</th>
</tr>
</thead>
<tbody><tr>
<td>constructor</td>
<td>non-constructor</td>
</tr>
<tr>
<td>인스턴스 생성o</td>
<td>인스턴스 생성x</td>
</tr>
<tr>
<td>생성자 함수로써 호출o</td>
<td>생성자 함수로써 호출x</td>
</tr>
<tr>
<td><strong>new</strong> 키워드 o</td>
<td><strong>new</strong> 키워드 x</td>
</tr>
<tr>
<td>prototype 프로퍼티o</td>
<td>prototype 프로퍼티x</td>
</tr>
<tr>
<td>[[HomeObject]]x</td>
<td>[[HomeObject]]o</td>
</tr>
<tr>
<td>super사용x</td>
<td>super사용o</td>
</tr>
</tbody></table>
<p>➖ 메서드는 일반함수와 달리 인스턴스를 생성할 수 없는 non-constructor라 생성자 함수로 사용 불가, prototype 프로퍼티도 없음, 프로토타입 생성도 안 함 </p>
<pre><code class="language-jsx">const obj= {
  x:1,
  bar: function(){return this.x;}, // 💩 프로퍼티 값으로 할당된 일반 함수
  foo(){return this.x;} // 👍 메서드 축약 표현으로 정의된 함수 
}

console.log(new obj.bar()); // bar {}    obj.bar는 일반 함수라 생성자 함수로써 호출 가능    
/** 생성자 함수를 호출해(new 키워드로) 새로운 빈 객체를 만듦
* 그리고 bar 함수 내에서 this로 사용
* 새 객체에는 x 프로퍼티가 없어 this.x는 undefined가 됨
* new 키워드 사용시 함수가 명시적으로 값을 반환하지 않으면, 새로 생성된 빈 객체를 반환함_객체를 생성한 생성자 함수의 이름을 붙여
*/
console.log(obj.bar()); // 1    this가 obj객체를 가리킴

console.log(new obj.foo()); // obj.foo is not a constructor    obj.foo는 메서드라 생성자 함수로써 호출 불가

console.log(obj.bar.hasOwnProperty('prototype')); // true 
// 일반함수는 생성자 함수로 사용 가능하고, prototype property 가짐 
// 생성자 함수는 prototype프로퍼티로 새로 생성된 객체의 프로토타입을 설정 -&gt; 객체 상속 가능 
console.log(obj.foo.hasOwnProperty('prototype')); // false   
// 메서드는 생성자 함수로 사용할 수 없고, prototype property 없음</code></pre>
<p>➖ 메서드는 상속 구조에서 <strong>부모 객체를 가리키는</strong><code>[[HomeObject]]</code>라는 내부 슬롯을 가져 <code>super</code> 키워드를 사용하여 부모 객체의 메서드와 생성자를 호출하는 등 객체와 더 밀접한 동작을 할 수 있음</p>
<pre><code class="language-jsx">// 💩 프로퍼티 값으로 할당된 일반 함수
const parent = {
  name: &quot;Parent&quot;,
  sayHi: function() {
    return `Hi! I'm ${this.name}.`;
  }
};

const child = {
  name: &quot;Child&quot;,
  // super 키워드 사용 불가
  sayHi: function() {
    // 부모의 sayHi 메서드를 호출하고 싶다면, 명시적으로 호출해야 함
    return `${parent.sayHi.call(this)} and this is ES5.`; //and this is ES5.
  }
};

console.log(child.sayHi()); // Hi! I'm Child and this is ES5.


// 👍 매서드 축약 표현으로 정의된 함수 
const child = {
  __proto__:parent, //parent 객체를 프로토타입으로 설정
  name: &quot;Child&quot;,
  // super 키워드 사용 가능
  // 부모 객체의 매서드 간단히 호출
  sayHi() {
    return `${super.sayHi()} and this is ES6.`; //and this is ES5.
  }
};

console.log(child.sayHi()); // Hi! I'm Child and this is ES5.</code></pre>
<blockquote>
<p>ES6 <strong>메서드</strong>는 본연의 기능<strong>super</strong> 을 추가하고 의미적으로 맞지 않는 기능<strong>constructor</strong> 는 제거함(객체의 동작을 정의하는 데만 사용되고, 객체 인스턴스를 생성하는 역할을 하지 않음) -&gt; 메서드를 정의할 때 프로퍼티 값으로 일반 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 게 좋음 </p>
</blockquote>
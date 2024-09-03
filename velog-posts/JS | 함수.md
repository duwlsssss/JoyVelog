<h3 id="✅-함수란">✅ 함수란</h3>
<p>프로그래밍 언어의 함수는 일련의 과정을 문statement로 구현하고, <strong>코드 블록으로 감싸 하나의 실행 단위</strong>로 정의한 것 </p>
<p>변수parameter: 함수 내부로 입력을 전달받는 변수
인수arguments: 입력(값으로 평가될 수 있는 표현식)
반환값return value: 출력 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/3be01fcf-bcc3-4eaf-8932-6719e7ab814b/image.png" /></p>
<h3 id="✅-함수-사용-이유">✅ 함수 사용 이유</h3>
<ul>
<li><strong>코드의 재사용</strong>
동일한 작업을 반복적으로 수행하면 그 코드는 함수로 만들어 사용하는 게 효율적</li>
<li><strong>유지보수의 편의성</strong>&amp;<strong>코드의 신뢰성</strong>
같은 코드의 중복으로 수정에 걸리는 시간, 사람의 실수 억제함</li>
<li><strong>코드의 가독성</strong>
적절한 함수 이름은 이름만으로 함수의 역할을 파악할 수 있게 도움 </li>
</ul>


<h3 id="✅-함수-정의-방법">✅ 함수 정의 방법</h3>
<ol>
<li><p>함수 선언문</p>
<pre><code class="language-jsx">function add(x, y) {
return x + y;
}</code></pre>
<p>함수 선언문으로 함수를 정의하는 경우, js엔진은 함수 선언문을 <code>함수 표현식</code>으로 변환해 <strong><code>함수 객체</code></strong>를 생성하고, <strong>함수 이름과 동일한 식별자</strong>가 <strong>암묵적으로 생성</strong>되고 함수 객체가 할당됨</p>
</li>
<li><p>함수 표현식</p>
<pre><code class="language-jsx">const add = function add(x, y) {
return x + y;
}</code></pre>
</li>
<li><p>Function 생성자 함수</p>
<pre><code class="language-jsx">const add = new Function(&quot;x&quot;, &quot;y&quot;, &quot;return x + y&quot;);</code></pre>
<p>빌트인 함수인 Function 함수에 매개변수 목록과, 함수 몸체를 문자열로 전달하며 new 연산자와 함꼐 호출하면 -&gt; 함수 객체를 생성해 반환함 </p>
</li>
<li><p>화살표 함수 (ES6)</p>
<pre><code class="language-jsx">const add = (x, y) =&gt; x + y;</code></pre>
<p>화살표fat arrow <code>=&gt;</code> 를 사용해 간략한 방법으로 함수 선언 가능, 항상 익명 함수로 정의함</p>
</li>
</ol>
<p>함수 표현식, Function 생성자 함수, 화살표 함수를 사용하면 <strong>변수 이름</strong>이 함수 객체를 참조함
</p>
<h3 id="✅-함수-호출">✅ 함수 호출</h3>
<p>JS에서 함수는 일급객체(값처럼 사용 가능)라 일반 객체와는 달리 <strong>호출</strong>이 가능
함수 이름이 아닌 <strong>함수 객체를 가리키는 식별자</strong>로 호출하는 것임
함수 호출: 인수를 매개변수를 통해 함수에 전달하며 함수의 실행을 명시적으로 지시
-&gt; 함수 호출시 코드블록에 담긴 문들이 일괄적으로 실행되고, 실행결과인 반환값을 반환함</p>
<pre><code class="language-jsx">function add(x, y) {
  return x + y;
}
add(1, 2);
console.log(x, y); // 💩 ReferenceError: x is not defined    함수의 매개변수의 스코프는 함수 내부임 

// 매개변수parameter와 인수arguments 개수는 일치하지 않아도 됨
// 매개변수의 개수 &gt; 인수의 개수 = 나머지 매개변수는 암묵적으로 undefined
function mul(x, y) {
  console.log(x, y); // 1 undefined
}
mul(1);

// 매개변수의 개수 &lt; 인수의 개수 = rest parameter(ES6)를 사용해 인수를 배열로 받음
function sub(x, y, ...rest) { // x,y를 제외한 나머지 인수들이 배열 형태로 rest에 담김
  console.log(rest); // [1]
  return x - y;
}
sub(3, 2, 1); // 1</code></pre>
<p>js의 함수는 <strong>매개변수와 인수의 개수 일치를 확인x</strong>, 동적 타입 언어라 <strong>매개변수의 타입을 사전에 지정 불가</strong>
-&gt; 함수를 정의할 때 인수가 제대로 전달됐는지 확인해야함</p>
<pre><code class="language-jsx">// 1️⃣ typeof 연산자로 arguments 문제 방지
function add(x, y) {
  if (typeof x !== &quot;number&quot; || typeof y !== &quot;number&quot;) {
    throw new TypeError(&quot;인수는 모두 숫자(number)값 이어야 합니다.&quot;);
  }

  return x + y;
}
console.log(add(1, 2)); // 3
console.log(add(2)); // TypeError: 인수는 모두 숫자(number)값 이어야 합니다.
console.log(add(&quot;a&quot;, &quot;b&quot;)); // TypeError: 인수는 모두 숫자(number)값 이어야 합니다.


// 2️⃣ &quot;단축 평가&quot;로 arguments 문제 방지
function mul(a, b, c) {
  a = a || 1;
  b = b || 1;
  c = c || 1;

  return a * b * c;
}
console.log(mul(1, 2, 3)); // 6
console.log(mul(1, 2)); // 2
console.log(mul(1)); // 1
console.log(mul()); // 1


// 3️⃣ parameter default value 설정으로 argument 문제 방지
function sub(a = 0, b = 0) {
  return a - b;
}
console.log(sub(10, 9)); // 1
console.log(sub(10)); // 10
console.log(sub()); // 0</code></pre>
<p>◽ 함수의 호출 방식
<strong>값에 의한 호출</strong>call by value: 함수 호출시 매개변수에 인수의 <strong>원시 값</strong>을 복사해서 전달
함수 안에서 인수의 값이 변경되어도(재할당) 원본은 변하지 않음_side effect x
<span style="color: grey;">+<strong>부수효과</strong>side effect: 함수로 들어온 인수의 상태를 변경하거나, 외부 상태를 변경하는 것_프로그램의 다른 부분에 영향을 미침</span></p>
<p><strong>참조에 의한 호출</strong>call by reference: 함수 호출시 매개변수에 인수의 <strong>참조값</strong>을 복사해 전달
함수 안에서 인수의 값이 변경되면(참조값을 통해 객체 변경) 원본이 훼손됨_side effect o
-&gt; 객체가 변경될 수 있어 <code>상태 변화 추적</code>이 어려움 </p>
<pre><code class="language-jsx">function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
  obj.gender = 'female';
}

let num = 100;
const obj = {
  name: 'Lee',
  gender: 'male'
};

console.log(num); // 100
console.log(obj); // Object {name: 'Lee', gender: 'male'}

changeVal(num, obj); // 원시값은 값 자체가 복사돼 전달되고, 객체는 참조값이 복사돼 전달됨

console.log(num); // 100    원시값은 원본 훼손x
console.log(obj); // Object {name: 'Kim', gender: 'female'}    💩 객체는 원본 훼손됨</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/426b1dd6-f3bd-42ae-8ddb-912caf02941e/image.png" /></p>
<p>✔ side effect 해결법
side effect를 줄이기 위해서는 객체를 <strong>불변 객체</strong>로 만들면 됨. <strong>깊은 복사</strong>를 통해 <strong>새로운 객체를 생성</strong>하고, <strong>원시값처럼 재할당</strong>하는 방법 사용
Object.freeze() 메서드를 재귀적으로 적용해서 객체의 모든 하위 객체까지 동결해 불변 객체를 만들 수 있음</p>
<pre><code class="language-jsx">// 입력 객체의 깊은 복사본을 생성해 반환하는 함수 
function deepFreeze(obj) {
  // 객체가 null이거나 객체가 아닌 경우 그대로 반환
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }

  // 모든 속성에 대해 동결을 시도하기 전. 속성이 객체나 배열인지 확인
  Object.keys(obj).forEach(key =&gt; {
    const value = obj[key];
    // 속성이 객체나 배열인 경우 재귀적으로 deepFreeze를 호출
    if (value &amp;&amp; typeof value === 'object') {
      deepFreeze(value);
    }
  });

  // 최종적으로 객체 자체를 동결
  return Object.freeze(obj);
}

const obj = { 
  name: &quot;Lee&quot;, 
  gender: 'male', 
};

// 객체 동결
deepFreeze(obj);

// 변경 시도
obj.name = &quot;Kim&quot;; // 변경되지 않음

console.log(obj); // { name: 'Lee', gender: 'male' }</code></pre>


<h3 id="✅-함수-형태">✅ 함수 형태</h3>
<ul>
<li><strong>즉시 실행 함수</strong>(IIFE, Immediately Invoked Function Expression)
: 정의되자마자 즉시 실행되는 함수 
단 한번만 호출됨, 익명 함수 사용이 일반적, 보통 클로저와 같이 사용
함수 이름이 없기 때문에 외부에서 호출하거나 참조할 수 없음 -&gt; 코드가 외부에 노출되지 않고 실행이 끝난 후 소멸되도록 함<pre><code class="language-jsx">// 그룹 연산자(...)로 감쌈, 함수이름이 없는 익명 함수를 사용하는 게 일반적임 
(function () {
...
})();
or
(function () {
...
}());</code></pre>
</li>
<li><strong>재귀 함수</strong>recursuve function
재귀 호출을 수행하는 함수, 자신을 무한 재귀 호출하므로 탈출 조건 필요<pre><code class="language-jsx">function factorial(n) {
if(n&lt;=1) return 1; // 탈출 조건: n이 1이하일 때 재귀 호출 멈춤
return n*factorial(n-1); // 자기 자신을 호출
}
</code></pre>
</li>
</ul>
<p>console.log(factorial(5)); // 120</p>
<pre><code>- **중첩함수**nested function === **내부함수**inner function
: 함수 내부에 정의된 함수
외부함수outer function: 중첩함수를 포함하는 함수
중첩함수는 자신을 포함한 외부함수를 돕는 **헬퍼 함수**helper function 역할을 함 
클로저를 생성해, 외부함수 종료 후에도 중첩함수가 외부함수의 변수에 접근 가능 
```jsx
function outer() {
  let x = 1;

  //중첩함수
  function inner() {
    let y = 2;
    console.log(x + y); // 3    외부 함수의 변수 사용 가능
  }

  inner(); // 중첩함수는 외부함수 내에서만 호출 가능
}

outer(); // 3</code></pre><ul>
<li><strong>콜백 함수</strong>callback function
: 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수 
고차함수<strong>H</strong>igher-<strong>O</strong>rder <strong>F</strong>unction: 콜백 함수를 전달받은 함수
고차함수는 콜백함수를 자긴의 일부분으로 합성함 
함수는 일급객체라 함수의 매개변수를 통해 함수를 전달 가능 -&gt; 함수는 내부로직에 강력히 의존하지 않고 <strong>외부</strong>에서 <strong>로직의 일부</strong>를 <strong>함수로 전달 받아 수행</strong>해 <strong><code>유연한 구조</code></strong>를 가짐 </li>
</ul>
<pre><code class="language-jsx">// 💩 반복하는 일을 공통으로 수행하지만 조건에 따라 로직이 조금씩 달라 매번 함수를 새롭게 정의해야함
// n만큼 어떤 일을 반복
function repeat1(n) {
  // i를 출력한다.
  for (let i = 0; i &lt; n; i++) console.log(i);
}
repeat1(5); // 0 1 2 3 4

// n만큼 어떤 일을 반복
function repeat2(n) {
  for (let i = 0; i &lt; n; i++) {
    // i가 홀수일 때만 출력한다.
    if (i % 2) console.log(i);
  }
}
repeat2(5); // 1 3

// 👍 함수를 합성해서 해결 가능, 공통 로직을 정의해 두고, 조건에 따라 변하는 로직은 추상화해 함수 외부에서 함수 내부로 전달함 
// 외부에서 전달받은 func 를 n만큼 반복 호출 - 고차 함수 repeat
function repeat(n, f) {
  for (let i = 0; i &lt; n; i++) {
    f(i);
  }
}

// 콜백 함수 정의 - logAll
const logAll = function (i) {
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logAll); // 0 1 2 3 4

// 콜백 함수 정의 - logOdd
const logOdd = function (i) {
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logOdd); // 1 3</code></pre>
<ul>
<li><strong>순수함수, 비순수함수</strong></li>
<li><em>순수 함수*</em>: 어떤 외부 상태(전역변수, 객체, DB 등)에도 의존하지 않고, 변경하지 않는, side effect가 없는 함수임 / 동일한 인수가 전달되면 언제나 동일한 결과 반환 =&gt; 예측 가능, 테스트 용이, 프로그램 안정성을 높임</li>
<li><em>비순수 함수*</em>: 외부 상태에 의존, 변경하는 함수, side effect 있음 / 동일한 인수가 전달되도 함수 외부 상태에 따라 다른 결과 반환할 수 있음  </li>
<li><code>함수형 프로그래밍</code>이란 순수 함수를 통해 외부 상태를 변경하는 <strong>부수효과를 최소화</strong>해서 <strong>불변성을 지향</strong>하는 프로그래밍 패러다임<pre><code class="language-jsx">let count = 0; // 현재 카운트를 나타내는 상태
</code></pre>
</li>
</ul>
<p>// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환함
function increase(n) {
  return ++n;
}</p>
<p>// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count); // 1</p>
<p>count = increase(count);
console.log(count); // 2</p>
<pre><code>
```jsx
let count = 0; // 현재 카운트를 나타내는 상태: increase 함수에 의해 변화함

// 비순수 함수
function increase() {
  return ++count; // 외부 상태에 의존하며 외부 상태를 변경함
}

// 비순수 함수는 외부 상태(count)를 변경하므로 상태 변화 추적 어려움
increase();
console.log(count); // 1

increase();
console.log(count); // 2</code></pre>

<blockquote>
<p>참고 자료</p>
</blockquote>
<ul>
<li><a href="https://m.yes24.com/Goods/Detail/92742567">https://m.yes24.com/Goods/Detail/92742567</a></li>
<li><a href="https://velog.io/@dev-redo/Javascript-%EC%88%9C%EC%88%98%ED%95%A8%EC%88%98%EC%99%80-%EB%B9%84%EC%88%9C%EC%88%98%ED%95%A8%EC%88%98">https://velog.io/@dev-redo/Javascript-%EC%88%9C%EC%88%98%ED%95%A8%EC%88%98%EC%99%80-%EB%B9%84%EC%88%9C%EC%88%98%ED%95%A8%EC%88%98</a></li>
</ul>
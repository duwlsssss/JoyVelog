<h3 id="✅-scope">✅ scope</h3>
<p>식별자가 유효한 범위
<strong>식별자의 선언 위치</strong>에 따라 다른 코드가 <strong>식별자를 참조할 수 있는 유효 범위</strong>가 결정됨 
js 엔진이 식별자를 검색할 때 사용하는 규칙 </p>
<h3 id="✅-식별자-결정">✅ 식별자 결정</h3>
<p>identifier resolution: js 엔진이 이름이 같은 두 식별자 중 어느 식별자를 참조해야할지 결정하는 것 
코드의 문맥context를 고려_코드가 어디서 실행되고 주변에 어떤 코드가 있는지(렉시컬 환경)에 따라 동일한 코드도 다른 결과를 만듦 </p>
<pre><code class="language-jsx">var x = &quot;global&quot;;

function foo() {
  var x = &quot;local&quot;;
  console.log(x); // local
}

foo();
console.log(x); // global</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/29200045-17e1-44bb-912c-87eeca4f7087/image.png" /></p>
<p>스코프 개념이 없으면 위 코드에서 x 변수가 전역, foo 함수 내에 모두 존재해 <strong>충돌</strong>할 것임</p>
<h3 id="✅-스코프-종류">✅ 스코프 종류</h3>
<p>변수는 자신이 선언된 위치에 따라 자신의 스코프가 결정됨 
전역에 선언된 변수는 전역 스코프_전역변수 -&gt; 어디서든지 참조 가능
지역에 선언된 변수는 지역 스코프_지역 변수 -&gt; 자신의 지역 스코프, 하위 지역 스코프에서 참조 가능 </p>
<h3 id="✅-스코프-체인">✅ 스코프 체인</h3>
<p>스코프는 함수의 중첩 의해 <code>계층적 구조</code>를 가짐 - 렉시컬 환경에 변수 식별자가 키로 등록되고, 변수에 할당이 일어나면 변수 식별자에 해당하는 값 변경함 </p>
<table>
<thead>
<tr>
<th><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/31c96a64-9a34-4b06-a9e3-dec015914fb4/image.png" /></th>
<th><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/4a892ea2-245a-42d3-8e3d-e52df7380c4b/image.png" /></th>
</tr>
</thead>
<tbody><tr>
<td>outer 함수가 만든 지역 스코프는 inner 함수가 만든 지역 스코프의 상위 스코프, outer 함수의 지역 스코프의 상위 스코프는 전역 스코프</td>
<td></td>
</tr>
<tr>
<td>-&gt; 모든 지역 스코프의 최상위 스코프는 전역 스코프</td>
<td></td>
</tr>
</tbody></table>
<p>변수 참조 시, js 엔진은 <strong>변수를 참조하는 코드의 스코프</strong>에서 <strong>시작</strong>해 <strong>스코프 체인</strong>을 통해 <strong>상위 스코프 방향으로 이동하며</strong> 선언된 변수를 검색함(identifier resolution)</p>
<blockquote>
<p><strong>스코프 체인</strong>은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결한chaining 것임</p>
</blockquote>
<p>▫ 스코프 체인에 의한 함수 검색</p>
<pre><code class="language-jsx">// 전역 함수 
function foo() {
  console.log(&quot;global function foo&quot;);
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log(&quot;local function foo&quot;);
  }

  foo(); // local function foo
}

bar();</code></pre>
<h3 id="✅-블록-레벨-스코프-vs-함수-레벨-스코프">✅ 블록 레벨 스코프 vs 함수 레벨 스코프</h3>
<p>블록 레벨 스코프 : 모든 코드 블록(함수, if, for, while, try/catch ...) 내에서 선언된 변수는 지역변수로 코드 블록 내에서만 유효함 _const, let
함수 레벨 스코프 : 함수 내에 선언된 변수는 지역변수로 함수 내에서만 유효함, 함수가 아닌 곳에서 선언된 var은 전역변수가 됨</p>
<h3 id="✅-렉시컬-스코프">✅ 렉시컬 스코프</h3>
<p>상위 스코프를 결정하는 방식 </p>
<ul>
<li>동적 스코프: <strong>어디서 호출했는지</strong>에 따라 상위스코프 결정
함수가 <strong>호출되는 시점</strong>에 동적으로 상위 스코프 결정 </li>
<li>정적 스코프(렉시컬 스코프): <strong>어디서 정의했는지</strong>에 따라 상위스코프 결정
동적 스코프처럼 <strong>상위 스코프가 동적으로 변하지 않고</strong>, <strong>함수 정의가 평가될때 정적으로 결정됨</strong>
js를 비롯한 대부분의 프로그래밍 언어가 렉시컬 스코프를 따름</li>
<li><blockquote>
<p>함수의 상위 스코프는 언제나 자신이 정의된 스코프 
함수가 호출될 때마다 함수의 상위스코프를 참조할 필요가 있기 때문에 *<em>함수 객체는 내부슬롯 Environment에 상위 스코프를 기억함 *</em></p>
</blockquote>
</li>
</ul>


<p>변수는 선언에 의해 <code>생성</code> -&gt; 할당을 통해 <code>값을 가짐</code> -&gt; 언젠가는 <code>소멸</code>함</p>
<p>변수는 자신이 등록된 스코프가 소멸(메모리 해제)될 때까지 유효함 
누군가 스코프를 참조하고 있으면 스코프는 소멸하지 않고 생존하게 됨 </p>
<p>변수에 생명 주기가 없다면 한 번 선언된 변수는 프로그램을 종료하지 않는 한 영원히 메모리 공간을 점유함</p>
<h3 id="✅-지역변수-생명-주기">✅ 지역변수 생명 주기</h3>
<p>지역 변수의 생명 주기는 <strong>함수의 생명주기와 일치해</strong>
함수가 호출돼 실행되는 동안에 유효함 === 지역변수는 지역 스코프를 가짐</p>
<pre><code class="language-jsx">function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined</code></pre>
<p><strong>지역 변수의 호이스팅</strong>은 <strong>지역 변수의 선언</strong>(선언+초기화)이 <strong>지역 스코프의 선두로 끌어올려진 것처럼</strong> 동작</p>
<pre><code class="language-jsx">var x = &quot;global&quot;;

function foo() {
  console.log(x); // undefined 
  var x = &quot;local&quot;;
}

foo(); // undefined 
console.log(x); // &quot;global&quot;    전역변수 x까지는 지역변수의 스코프가 유효하지 않음 </code></pre>
<h3 id="✅-전역-변수-생명-주기">✅ 전역 변수 생명 주기</h3>
<p><code>var</code> 키워드로 선언한 <strong>전역 변수의 생명 주기</strong>는 <strong><a href="https://velog.io/@kimlj0814/JS-%EB%B9%8C%ED%8A%B8%EC%9D%B8-%EA%B0%9D%EC%B2%B4">전역 객체</a>의 생명 주기</strong>와 일치함 </p>
<p>전역 코드는 명시적 호출 없이 로드 되자마자 실행되고, 더 이상 실행할 코드가 없으면 종료됨 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/d57ac79a-c6b0-4a9c-b5a5-260263d789f8/image.png" /></p>
<h3 id="✅-전역-변수의-문제점">✅ 전역 변수의 문제점</h3>
<ul>
<li>암묵적 결합: 모든 코드가 전역 변수 참조하고 변경 가능 </li>
<li>긴 생명 주기: 메모리 리소스를 오랜 기간 소비</li>
<li>스코프 체인 상의 종점에 존재: 전역 변수의 검색 속도가 가장 느림</li>
<li>네임 스페이스 오염: js는 하나의 전역 스코프를 공유해 같은 이름의 전역변수,전역함수로 예상치 못한 결과 초래</li>
</ul>
<h3 id="✅-전역-변수-사용-억제-방법">✅ 전역 변수 사용 억제 방법</h3>
<ol>
<li>즉시 실행 함수로 감싸기
모든 코드를 즉시실행함수로 감싸 모든 변수를 즉시실행함수의 지역변수로 만듦
즉시실행함수 호출이 종료되면 내부에 있던 모든 변수가 소멸됨(일회성)<pre><code class="language-jsx">(function () {
var foo = 100; // 즉시 실행 함수의 지역 변수
// ...
})();
console.log(foo); // ReferenceError: foo is not defined</code></pre>
</li>
<li>모듈 패턴 사용
<code>class</code>처럼 즉시실행함수로 변수와 함수를 감싸 <strong>하나의 모듈로 캡슐화함</strong>
<a href="https://velog.io/@kimlj0814/JS-%ED%81%B4%EB%A1%9C%EC%A0%80">클로저</a>를 활용하여 내부 상태를 은닉하면서 공용 API를 제공
```jsx
/**</li>
</ol>
<ul>
<li><p>Counter 변수에는 즉시 실행함수로 생성된 클래스 느낌의 객체가 값으로 할당되어 있음</p>
</li>
<li><p>즉시 실행 함수 내부에서 선언되어 있는 변수는 private 한 성질을 가져, 외부에서 참조 불가능</p>
</li>
<li><p>반환하는 public 한 성질의 메서드들은 외부에서 특정 기능에만 접근할 수 있게 함 </p>
</li>
<li><p>/
var Counter = (function () {
 // private 변수
 var num = 0;</p>
<p> // 외부로 공개할 데이터나 메서드 프로퍼티를 추가한 객체를 반환
 return {
   increase() {</p>
<pre><code> return ++num;</code></pre><p>   },
   decrease() {</p>
<pre><code> return --num;</code></pre><p>   },
 };
})();</p>
</li>
</ul>
<p>console.log(Counter.num); // undefined    private 변수는 외부로 노출되지 X
console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0</p>
<p>```
3. <a href="https://velog.io/@kimlj0814/JS-%EB%AA%A8%EB%93%88">모듈</a> (ES6)
파일 자체의 <strong>독자적인 모듈 스코프</strong>를 제공
모듈 내에서 var로 선언한 변수는 전역변수, window객체의 프로퍼티가 아님</p>
<blockquote>
<p>참고 자료</p>
</blockquote>
<ul>
<li><a href="https://m.yes24.com/Goods/Detail/92742567">https://m.yes24.com/Goods/Detail/92742567</a></li>
<li><a href="https://github.com/youngminss/Docs-modernJS__deepDive/tree/master/10-Scope">https://github.com/youngminss/Docs-modernJS__deepDive/tree/master/10-Scope</a></li>
</ul>
<h3 id="✅-타입체크">✅ 타입체크</h3>
<p>js는 동적 타입 언어라 엄격한 타입 체크 필요 </p>
<p>▫ <strong>typeof</strong>
typeof 연산자로 변수를 연산하면 <strong>변수에 할당된 값의 데이터 타입</strong> 문자열을 반환함
primitive type values는 typeof로 잘 검사되는데
reference type values는 typeof로 검사하는 데 한계 있음_세부 유형을 구분 못함</p>
<pre><code class="language-jsx">//primitive type values
typeof '문자열' // &quot;string&quot;
typeof true // &quot;boolean&quot;
typeof Symbol() // &quot;symbol&quot;

//reference type values
typeof {}; // &quot;object&quot;
typeof function() {}; // &quot;function&quot;
typeof []; // &quot;object&quot;
typeof new Date(); // &quot;object&quot;
typeof new String('문자열'); // &quot;object&quot;</code></pre>
<blockquote>
<p>reference type을 확인할 때 사용할 대안</p>
</blockquote>
<p>▫ <strong>Object.prototype.toString.call() **
**객체</strong>의 타입을 나타내는 문자열 반환</p>
<pre><code class="language-jsx">Object.prototype.toString.call([]); // &quot;[object Array]&quot;
Object.prototype.toString.call({}); // &quot;[object Object]&quot;
Object.prototype.toString.call(function() {}); // &quot;[object Function]&quot;
Object.prototype.toString.call(new Date()); // &quot;[object Date]&quot;
Object.prototype.toString.call(new String('문자열')); // &quot;[object String]&quot;</code></pre>
<p>+instanceof도 typeof의 대안으로 많이 말하는데 얘는 <code>객체</code>의 프로토타입 체인에 <code>특정 생성자</code>가 있는지 확인함  </p>
<pre><code class="language-jsx">[] instanceof Array // true    배열객체의 프로토타입 체인에 Array.prototype이 있음</code></pre>
<p>그러나 모든 객체는 Object.prototype을 상속받아
<code>객체</code> instanceof <code>Object</code> 하면 true가 
나와 엄격한 타입 체크가 안됨</p>
<pre><code class="language-jsx">[] instanceof Object; // true
function() {} instanceof Object; // true</code></pre>


<h3 id="✅-타입-변환">✅ 타입 변환</h3>
<ul>
<li>명시적 타입 변환, 암묵적 타입 변환
원시값을 직접 변경하는 건 아님, 기존 원시 값을 사용해 다른 타입의 새로운 원시값을 생성하는 것임 <pre><code class="language-jsx">let x = 1;
let str = x + &quot;&quot;; // x는 원시값이고 그 자체가 변하는 게 아니라, 표현식을 평가하기 위해 암묵적으로 새 문자열 &quot;1&quot;을 생성해 사용, 표현식 평가가 끝나면 가비지 콜렉터에 의해 메모리에서 해제됨   
</code></pre>
</li>
</ul>
<p>console.log(typeof str, str); // string &quot;1&quot;
console.log(typeof x, x); // &quot;number&quot; 1</p>
<pre><code>
### ✅ implicit coercion(type coericion)
개발자의 의도와 상관없이 js엔진이 코드의 문맥에 따라 **암묵적으로 타입을 변환**하는 것_primitive type으로
```jsx
// 문자열 타입으로 변환
NaN + '';             // &quot;NaN&quot; string 표현식이 평가되는 일시적인 순간에만 NaN이 &quot;NaN&quot;으로 취급됨
1 + true; // 2 number
1 + null; // 1 number (1+0)
1 + undefined; // NaN (1+NaN)

// 숫자 타입으로 변환
+&quot;&quot;; // 0  빈 문자열
+[]; // 0  빈 배열
+false; // 0
+null; // 0
+{}; // NaN   
+function () {}; // NaN  객체
+[10, 20]; // NaN  값이 2개 이상인 배열
+undefined; // NaN

// 불리언 타입으로 변환
// 💡 js 엔진은 불라언 타입이 아닌 값을 Truthy 값, Falsy 값으로 구분함, Thruthy값은 true로 Falsy값은 false로 암묵적 타입 변환함
// Falsy 값으로 판단하는 값
false, null, undefined, ‘’, 0, -0, NaN
// 나머진 Thruthy 값으로 판단

if(!null) console.log('null is falsy value');</code></pre><blockquote>
<p>개발자는 자신의 코드에서 암묵적 타입 변환이 발생하는지, 발생한다면 어떤 타입으로 변환되는지, 타입 변환된 값으로 인해 표현식이 어떻게 평가될 것인지 예측할 수 있어야 함, 예측이 결과와 일치하지 않으면 오류 발생 가능</p>
</blockquote>
<blockquote>
<p>js가 아닌 개발자가 직접 타입 변환을 하는 명시적 형변환 지향하자</p>
</blockquote>
<h3 id="✅-explicit-coerciontype-casting">✅ explicit coercion(type casting)</h3>
<p>개발자의 의도에 따라 명시적으로 타입을 변경하는 방법</p>
<ul>
<li>표준 빌트인(built-in) 생성자 함수 → String(), Number(), Boolean() 을 new 키워드 없이 호출하는 방법</li>
<li>빌트인(built-in) 메서드를 사용하는 방법</li>
<li>암묵적 타입변환 </li>
</ul>
<pre><code class="language-jsx">// 문자열 타입으로 변환
String(5) // &quot;5&quot;
(5).toString() 
5+'' 

// 숫자 타입으로 변환
Number(&quot;5&quot;); // 5
parseInt(&quot;5&quot;)
+&quot;5&quot;

// 불리언 타입으로 변환
Boolean({}) // true
!!{}; // 부정 논리 연산자를 두번 사용 </code></pre>
<h3 id="✅-동등비교-vs-일치비교">✅ 동등비교 vs 일치비교</h3>
<p><code>동등 비교</code>loose equality (==,eqeq) : 느슨한 동등연산자. 암묵적 타입 변환을 통해 좌항과 우항의 피연산자 타입을 일치시킨 후, 값이 같은지 비교 </p>
<pre><code class="language-jsx">5 == 5; // true

// 타입은 number 와 string 으로 다르지만, &quot;암묵적 타입 변환&quot;을 통해 먼저 타입을 일치시키고 비교함
5 == &quot;5&quot;; // true</code></pre>
<p><code>일치 비교</code>strict equality (===,eqeqeq) : 엄격한 동등연산자, 좌항과 우항의 피연산자가 타입도, 값도 같은지 비교</p>
<pre><code class="language-jsx">5 === 5; // true

// 암묵적 타입 변환을 하지 않고 값,타입 둘다 비교
5 === &quot;5&quot;; // false</code></pre>
<blockquote>
<p>==은 예측 못한 결과 만들어내 위험함 ===(일치 비교 연산자) 사용 지향하자</p>
</blockquote>
<h3 id="✅-값">✅ 값</h3>
<p><code>값</code>value은 <code>표현식</code>expression이 <code>평가돼</code>evaluate 생성된 결과</p>
<h3 id="✅-리터럴">✅ 리터럴</h3>
<p><code>리터럴</code>lliteral은 사람이 이해할 수 있는 문자 or 약속된 기호를 사용해 값을 생성하는 <code>표기법</code>notation</p>
<p>js 엔진은 런타임에 리터럴을 평가해 값을 생성함 </p>
<pre><code class="language-jsx">3; // 숫자 리터럴인 3을 코드에 쓰면 js엔진이 평가해 숫자값 3을 생성</code></pre>
<h3 id="✅-표현식">✅ 표현식</h3>
<p><code>표현식</code>expression은 <strong>값으로 평가될 수 있는</strong> <code>문</code>statement </p>
<p>표현식이 평가되면 새로운 값을 생성 or 기존 값을 참조함</p>
<pre><code class="language-jsx">var score = 50+50 // score, 50+50은 모두 표현식임</code></pre>
<h3 id="✅-문">✅ 문</h3>
<p><code>문</code>statement은 프로그램을 구성하는 기본 단위, 최소 실행 단위</p>
<p>문법적의미를 가지고, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소인 <strong>토큰</strong>으로 구성됨_키워드 식별자 연산자 ; , .</p>
<p>문은 <code>값으로 평가될 수 있는 표현식인 문</code>, <code>값으로 평가될 수 없는 표현식이 아닌 문</code>으로 나뉨 </p>
<pre><code class="language-jsx">var a; // 변수 선언문 = &quot;값&quot;으로 평가될 수 X -&gt; 표현식 X -&gt; 변수에 할당 불가
a = 1 + 2; // js엔진이 런타임 시점에 숫자값 3을 생성하므로 표현식 -&gt; 변수에 할당 가능 </code></pre>
<h3 id="✅-데이터-타입">✅ 데이터 타입</h3>
<p>값의 종류 / js의 모든 값은 데이터 타입을 가짐 </p>
<p>데이터 타입이 필요한 이유: 값을 저장할 때 <strong>확보해야 하는 메모리 공간의 크기</strong>결정 필요_js엔진은 데이터 타입에 따라 정해진 크기의 메모리 공간을 확보함 / 값을 참조하려면 <strong>한번에 읽어야 하는 메모리 공간의 크기</strong>(메모리 셀의 개수)를 결정해야 함 / 메모리에서 읽어들인 <strong>2진수를 어떻게 해석할지</strong> 결정해야 함_숫자or문자열</p>
<blockquote>
<p>ES6가 제공하는 데이터 타입</p>
</blockquote>
<ul>
<li><p>Primitive Type(immutable value): number, bigint, string, boolean, undefined, null, symbol</p>
</li>
<li><p>Reference Type: Object(Function, Array, Date, RegExp ...)</p>
</li>
<li><p>숫자 타입</p>
</li>
</ul>
<p>자바스크립트에선 숫자를 <strong>64비트 2진 부동소수점 타입</strong>으로 사용함. 모든 수를 실수로 처리하고 정수만 표현하기 위한 데이터 타입이 없음. 
64비트 중 1비트는 부호, 11비트는 지수, 그리고 52비트는 유효숫자를 나타냄. 여기서 유효숫자는 hidden bit를 포함해 <strong>사실상 53비트</strong>의 정밀도를 가짐. 이는 유효숫자의 범위가 일반적으로 1.0 이상 2.0 미만이기 때문에 최상위 비트가 항상 1로 가정돼 저장되기 때문. 이 덕분에 유효숫자를 표현할 때 최상위 비트를 생략하고도 정확성을 유지할 수 있음</p>
<p>실수 외에도 Infinity, -Infinity, NaN 도 표현 가능 </p>
<p>+자바스크립트는 10진 소수값을 제대로 처리하지 못함
js에서 숫자를 64비트 2진 부동소수점 형식으로 사용하고 일부 10진 소수를 2진수로 변환할 때 <strong>무한소수</strong>로 표현되고 이를 비트 수에 맞춰 <strong>근사값</strong>으로 바꾸기 때문</p>


<ul>
<li>문자열 타입</li>
</ul>
<p><code>‘’</code>** or <code>””</code> or <code>백틱(\`\`)</code> 으로 텍스트를 감싸서 사용
</p>
<p>◽ 템플릿 리터럴</p>
<p>ES6부터 도입된 <strong>내장된 <code>표현식</code>을 허용하는 문자열 리터럴</strong> / 백틱(``)으로 표현 / <code>표현식 삽입</code>expression interpolation,  <code>멀티라인 문자열</code>multi-line strings,  <code>태그드 템플릿</code>tagged templates 등 편리한 문자열 처리 기능 제공 </p>
<p>템플릿 리터럴은 런타임에 <strong>일반 문자열</strong>로 변환돼 처리됨</p>
<p>-표현식 삽입
${}를 사용해 표현식을 일반 문자열에 삽입함 
ㄴ안의 내용은 문자열로 강제 타입 변환돼 삽입됨</p>
<pre><code class="language-jsx">var first = &quot;YOUNG&quot;;
var last = &quot;MIN&quot;;

// ES5: 문자열 연결
var classes = &quot;header&quot;;
classes += isLargeScreen()
  ? &quot;&quot;
  : item.isCollapsed
    ? &quot; icon-expander&quot;
    : &quot; icon-collapser&quot;;

// ES6: 표현식 삽입
const classes = `header ${
  isLargeScreen() ? &quot;&quot; : `icon-${item.isCollapsed ? &quot;expander&quot; : &quot;collapser&quot;}`
}`;</code></pre>
<p>-멀티라인 문자열
escape sequence 대신 사용 가능</p>
<pre><code class="language-jsx">// ES5: 문자열 연결
var template = '&lt;ul&gt;\n\t&lt;li&gt;&lt;a href=&quot;#&quot;&gt;Home&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;';

// ES6: 표현식 삽입
var template = `&lt;ul&gt;
    &lt;li&gt;&lt;a href=&quot;#&quot;&gt;Home&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;;

/*
&lt;ul&gt;
  &lt;li&gt;&lt;a href=&quot;#&quot;&gt;Home&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;
*/</code></pre>
<p>-태그드 템플릿</p>
<p>템플릿 리터럴 앞에 <strong>태그 함수</strong>를 붙여 함수를 호출하며 템플릿 리터럴의 각 부분(순수 문자열 부분,표현식 부분)을 인수로 전달함 </p>
<pre><code class="language-jsx">function template(strings, ...keys) { // strings=[&quot;&quot;, &quot; &quot;, &quot;!&quot;], keys=[0, &quot;foo&quot;] 템플릿 리터럴 중 표현식을 제외한 문자열들이 담긴 배열, 표현식이 담긴 배열 
    return function (...values) { // values=[&quot;Hello&quot;, {foo: &quot;World&quot;}]
        var dict = values[values.length - 1] || {}; // dict={foo: &quot;World&quot;}
        var result = [strings[0]]; // result=[&quot;&quot;]
        keys.forEach(function (key, i) {
            var value = Number.isInteger(key) ? values[key] : dict[key];
            result.push(value, strings[i + 1]);
        });
        return result.join(&quot;&quot;);
    };
}

var tClosure = template `${0} ${&quot;foo&quot;}!`; // 태그 함수인 template에 템플릿 리터럴을 넘김
tClosure(&quot;Hello&quot;, { foo: &quot;World&quot; }); // &quot;Hello World!&quot;</code></pre>


<ul>
<li>불리언 타입</li>
</ul>
<p>불리언 타입의 값은 논리적 참,거짓을 나타내는 true, false 뿐임 </p>


<ul>
<li>undefined 타입</li>
</ul>
<p>undefined 타입의 값은 <code>undifined</code> 뿐임   </p>
<p>변수 선언 후 값을 할당하지 않은 경우 js엔진에 의해 자동으로 할당됨_값&amp;데이터타입이 모두 없음===값&amp;데이터타입이 undefined
전역 객체의 속성 중 하나로, 전역 스코프에서 변수처럼 사용 가능, 기본적으로 모든 변수의 초기값은 undefined임
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/c3f6de94-950f-46fc-984b-9d0c264c01f0/image.png" /></p>
<blockquote>
<p>undefined를 변수명으로 사용가능해서 유지보수와 디버깅에 문제가 생길 수 있으니 사용 지양하자</p>
</blockquote>
<ul>
<li>null 타입</li>
</ul>
<p>null 타입의 값은 <code>null</code>뿐임   </p>
<p>변수의 값이 의도적으로 비어있음을 나타냄(intentional absence)_변수는 빈 값(null)을 할당된 빈 객체(object)<span style="color: grey;">-js 언어적 오류</span>
변수에 null을 할당하면 변수가 이전에 참조하던 값을 더 이상 참조하지 않겠다는 뜻임, js 엔진은 누구도 참조하지 않는 메모리 공간에 가비지 콜렉션을 수행함 </p>
<p>◽ undifined vs null
undefined는 아직 값이 할당되지 않은 상태, null은 의도적으로 비어있는 상태 </p>
<pre><code class="language-jsx">let a;
console.log(a); // undefined
console.log(typeof a); // &quot;undefined&quot;

let b = null;
console.log(b); // null
console.log(typeof b); // &quot;object&quot; 

// null은 숫자 타입과 연산될 때 0으로 취급됨
console.log(null + 123); // 123
console.log(isNaN(1 + null)); // false

// undefined는 숫자 타입과 연산될 때 NaN(Not a Number)으로 취급됨
console.log(a + 10); // NaN
console.log(isNaN(1 + undefined)); // true</code></pre>


<ul>
<li>심볼 타입</li>
</ul>
<p>ES6부터 도입된 변경 불가한 원시 타입의 값임 </p>
<p>심볼 값은 외부에 노출되지 않으며, 다른 값과 중복되지 않는 유일한 값이므로 <strong>객체의 유일한 프로퍼티 키를 만들기 위해 사용함</strong>_프로퍼티 키의 충돌 방지 </p>
<p>Symbol 함수를 호출해 생성함</p>
<pre><code class="language-jsx">var key = Symbol('key'); // Symbol 함수를 호출할 때 인자로 전달하는 문자열 값은 심볼에 대한 설명(Description)으로, 디버깅 용도로만 사용되고 심볼의 고유성엔 영향 x
console.log(typeof key); // &quot;symbol&quot;

const sym1 = Symbol();
const sym2 = Symbol();
console.log(sym1 === sym1);  // true
console.log(sym1 === sym2);  // false, Symbol 함수를 호출하면 매번 새로운(고유한) 심볼이 생성됨

// 객체 생성
var obj = {};

// 이름이 충돌할 위험이 없는 유일무이한 값인 심볼을 &quot;프로퍼티 키&quot;로 사용
obj[key] = 'value';
console.log(obj[key]);  // 'value'</code></pre>
<h3 id="✅-동적-타입-언어">✅ 동적 타입 언어</h3>
<p>js는 변수 선언시 <strong>타입을 선언하지 않음</strong></p>
<p>dynamic typing: js 변수는 선언이 아닌 <strong>할당에 의해 타입이 결정되고</strong>(타입추론type inference) <strong>재할당에 의해 변수의 타입이 동적으로 변할</strong> 수 있는 특성 가짐</p>
<p>변수는 타입을 갖지 않고 <strong>값이 타입을 가짐</strong></p>
<blockquote>
<p>high flexibility low reliability - 의도치 않게 타입이 자동으로 변환돼 오류 발생 가능 -&gt; 타입 검사 주의해야함</p>
</blockquote>
<p>프로젝트로 개발을 시작해 기본기가 없다고 느껴 &lt;모던 자바스크립트 Deep Dive&gt; 책과 udemy의 &lt;클린코드 자바스크립트&gt; 강의와 MDN 문서를 참고해 자바스크립트를 깊게 공부하고 그 과정을 벨로그에 하나하나 기록해보려합니다.</p>
<p>첫 게시물로 변수 생성 과정과 호이스팅을 중심으로 공부한 내용을 정리해보겠습니다.
</p>
<ul>
<li><p>식별자
어떤 값을 식별할 수 있는 고유한 이름 === 메모리 주소에 붙인 이름 
식별자는 메모리 주소를 기억함, 식별자와 값이 든 메모리 주소는 매핑관계, 매핑 정보도 메모리에 저장됨 
변수, 함수, 클래스 등의 이름은 모두 식별자_메모리에 존재하는 어떤 값을 식별할 수 있는 이름은 모두 식별자라고 함</p>
</li>
<li><p>스코프
식별자가 유효한 범위, 식별자의 선언 위치에 따라 범위가 달라짐 
전역에 선언된 변수는 전역 스코프 / 지역에 선언된 변수는 지역 스코프를 가짐</p>

### ✅ 변수

</li>
</ul>
<p>: 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 || 그 메모리 공간의 상징적 이름 </p>
<pre><code class="language-jsx">const age = 50; </code></pre>
<p>변수 age에 50을 저장한 <strong>메모리 주소</strong>를 저장, 이후 age를 사용하면 메모리 주소와 매핑된 공간에 <strong>저장된 값</strong>인 50을 반환함
</p>
<h3 id="✅-변수-생성-과정">✅ 변수 생성 과정</h3>
<p>변수는 생성시 <strong>선언, 초기화, 할당</strong> 3단계를 거친다</p>
<ol>
<li>선언 단계: 변수가 Lexical Environment의 Environment Record에 등록됨, 자바스크립트 엔진은 변수의 이름을 인식하고, 변수가 해당 스코프 내에서 존재하게 됨</li>
<li>초기화 단계: 변수 객체에 등록된 변수에 메모리가 할당됨_js엔진이 암묵적으로 undefined를 할당해 초기화</li>
<li>할당 단계: undefined로 초기화된 변수에 실제 값을 할당</li>
</ol>
<p>+초기화 단계가 있는 이유는 확보된 메모리 공간에 이전에 다른 애플리케이션에서 사용한 값이 남아있을 수 있는데 (garbage value) 만약 메모리 할당 후 값이 할당되지 않은 상태에서 변수값을 참조하면 쓰레기 값이 나올 수 있음. var 키워드는 암묵적으로 초기화를 해 이런 위험으로부터 안전함 </p>
<p>
변수 선언시 var, let, const 키워드 사용 </p>
<p><strong>var: 함수 레벨 스코프</strong> / 함수 내에 선언된 변수는 <strong>지역변수로 함수 내에서만 유효함</strong>, 함수가 아닌 곳에서 선언된 var은 전역변수가 됨</p>
<pre><code class="language-jsx">if (true) {
    var x = 10;
}
console.log(x); // 10_블록 바깥에서도 접근 가능</code></pre>
<p>재할당,재선언 가능</p>
<pre><code class="language-jsx">var name = '이름1';
var name = '이름2';
var name = '이름3';

console.log(name); //이름3</code></pre>
<p>같은 이름으로 재선언과 재할당을 했지만 에러가 발생하지 않고 가장 마지막에 선언,할당된 ‘이름3’이 출력됨 </p>
<p>=&gt; 의도치 않은 전역변수 선언으로 인해 예측 어려운 버그 발생(변수 이름 중복, 값 변경, 메모리 리소스 낭비...😨)</p>
<p><strong>const, let: 블록 레벨 스코프</strong> / 모든 코드 블록(함수, if, for, while, try/catch ...) 내에서 선언된 변수는 <strong>지역변수로 코드 블록 내에서만 유효함</strong></p>
<p>ES6에서 도입된 키워드임
<strong>T</strong>emporal <strong>D</strong>ead <strong>Z</strong>one 가짐</p>
<p>let은 블록스코프 내 재할당 가능, 블록이 다르면 재선언도 가능</p>
<pre><code class="language-jsx">{
    let x = 10; // 블록 스코프 내에서 변수 x 선언
    console.log(x); // 10

    x = 20; // 같은 블록 내에서 재할당
    console.log(x); // 20

    // 같은 블록 내에서 재선언하면 에러가 발생
    let x = 30; // Uncaught SyntaxError: Identifier 'x' has already been declared
}

{
    // 다른 블록 내에선 변수 x를 재선언 가능
    let x = 30; 
    console.log(x); // 30
}</code></pre>
<p>const는 재할당, 재선언 불가, 선언시 초기화해야함 / const 키워드로 선언된 변수에 원시 값을 할당하면 값을 변경할 수 없으나, 배열이나 객체 등 참조형 데이터를 할당한 경우 속성 변경은 가능_*<em>재할당을 금지한거지 불변은 아님 *</em></p>
<pre><code class="language-jsx">const person = { 
    name: 'kim', 
    age: '7', 
};

person.name='lee';
console.log(person); // {name:'lee', age:'7'}</code></pre>
<p>=&gt; 전역 변수를 남발할 가능성이 높았던 var의 한계를 개선해 <strong>코드의 안정성과 가독성을 높임</strong> but let은 재할당은 가능해 의도치 않은 값 변경 가능성 있음</p>


<p>+변수의 재할당
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/4b2cdc05-8d0d-485e-8266-eea6310b67c6/image.png" /></p>
<p>재할당시 이전 값은 어떤 식별자와도 연결되지 않고, 불필요해짐, 이런 값들은 가비지 콜렉터에 의해 메모리에서 자동 해제됨(언제 해제될진 예측 불가)_가비지 콜렉터는 애플리케이션이 할당한 메모리 공간을 주기적으로 검사해 더 이상 사용되지 않는 메모리(어떤 식별자도 참조하지 않는 메모리 공간)를 해제하는 기능임 → 메모리 누수 방지</p>


<h3 id="✅-호이스팅">✅ 호이스팅</h3>
<p>: 인터프리터가 코드를 실행하기 전 변수나 함수 <strong>선언문</strong>이 코드의 최상단으로 끌어올려진 것처럼 동작하는 js의 특징</p>
<pre><code class="language-jsx">console.log(foo); // undefined
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;</code></pre>
<p>var 키워드로 선언한 변수는 <strong>런타임 이전에 선언과 초기화가 한번에 일어나</strong> 선언문 전에 접근해도 undefined가 출력됨, 이후 변수 할당문에서 값이 할당됨
<strong><em>변수 선언은 런타임 이전에 실행됨, 값 할당은 런타임에 실행됨</em></strong>
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/94c2373e-8eec-4f4b-baf4-fd9cc943dcfb/image.png" /></p>

let, const는 호이스팅되지 않는 것처럼 보이지만

<pre><code class="language-jsx">const x = 1;
{
  console.log(x); // ReferenceError
  const x = 2;
}</code></pre>
<p>위 코드에서 호이스팅이 일어나지 않는다면 참조에러가 아닌 1을 반환했을 것임. const x =2; 가 호이스팅돼 TDZ에 머무르기 때문에 ReferenceError가 발생하는 것임 </p>
<p>let, const는 선언은 호이스팅되나 초기화는 선언문에서 실행돼 선언문 전에 접근시 ReferenceError 발생_TDZ의 영향을 받는 것임 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/5f8ed1ee-8d91-432d-a0e5-fc42e73fd4ba/image.png" /></p>
<p><strong>TDZ</strong>: 변수 선언 시점(스코프 시작 지점)부터 초기화 시작 시점까지의 구간, 이 구간 중 변수를 참조하면 ReferenceError 발생_초기화 이전이므로 변수에 메모리 공간이 확보되지 않은 상태이기 때문</p>
<p><strong>TDZ로 코드의 안전성이 높아지고, 호이스팅 때문에 의도치 않은 값이 노출되는 걸 방지할 수 있음, 선언된 변수가 초기화되기 전 접근되는 걸 막아줌</strong></p>
<p>함수 선언문도 호이스팅됨_선언,초기화,할당 동시 진행
but 함수 표현식(변수에 할당된 함수 리터럴)은 변수 호이스팅만 발생하고 함수 자체는 런타임에 평가돼 초기화와 할당이 함수 표현식이 실행되는 시점에 이루어짐. 그래서 함수가 정의된 후에만 호출 가능
=&gt; 함수 선언문은 호이스팅으로 함수 선언 전 호출을 가능하게 해 코드의 흐름을 예측하기 어렵게 만들 수 있음. 코드의 가독성과 유지보수성을 높이기 위해 함수 선언문보다 함수 표현식을 사용하는 것이 더 좋음</p>
<pre><code class="language-jsx">//함수 참조
console.dir(plus); 
console.dir(minus); // undifind

//함수 호출
console.log(plus(10,5));// 15
console.log(minus(10,5));// Uncaught TypeError: minus is not a function

//함수 선언문 
funtion plus(a,b){
 return(a+b);
}

//함수 표현문
var minus = funtion(a,b){
return (a-b);
}</code></pre>
<p><span style="color: blue;">==&gt; 전역변수는 위험하니까 만들지 마셈 / 지역변수 만들기 / 변수 선언엔 기본적으로 const 사용, 재할당이 필요할 경우 let 사용(스코프 최대한 좁히기) / 호이스팅 방지하기 위해 함수 표현식 지향</span></p>
<h3 id="💡-this">💡 this</h3>
<p>객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조입니다.</p>
<p>객체 내부의 메서드는 자신이 속한 객체를 가리키는 특별한 식별자인 this를 사용할 수 있습니다.</p>
<p><code>this</code>는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 <code>자기 참조 변수</code>(self-referencing variable) 입니다. </p>
<p>this에 바인딩되는 값은 함수 호출 방식에 의해 동적으로 결정됩니다.</p>
<h3 id="✅-함수-호출방식에-따른-this-바인딩">✅ 함수 호출방식에 따른 this 바인딩</h3>
<table>
<thead>
<tr>
<th>함수 호출 방식</th>
<th>this 바인딩</th>
</tr>
</thead>
<tbody><tr>
<td>일반 함수 호출</td>
<td>전역 객체</td>
</tr>
<tr>
<td>메서드 호출</td>
<td>메서드를 호출한 객체</td>
</tr>
<tr>
<td>생성자 함수 호출</td>
<td>생성자 함수가 (미래에) 생성할 인스턴스</td>
</tr>
<tr>
<td>Function.prototype.apply/call/bind 메서드에 의한 간접 호출</td>
<td>Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달할 객체</td>
</tr>
</tbody></table>
<pre><code class="language-js">const foo = fuction() {
    console.dir(this);
}

// 1️⃣ 일반 함수 호출
foo(); // window

// 2️⃣ 메서드 호출
// foo 프로퍼티에 함수 객체를 할당해 호출
const obj = { foo };
obj.foo(); //obj

// 3️⃣ 생성자 함수 호출
new foo(); // foo {}

// 4️⃣ Function.prototype.apply/call/bind 메서드에 의한 간접 호출
const bar = { name: 'bar' };

foo.apply(bar); //bar
foo.call(bar); //bar
foo.bind(bar); //bar</code></pre>
<pre><code class="language-js">// ✚ 메서드 내의 중첨함수여도 일반함수로 호출되며녀 this가 전역 객체로 바인딩된다.
const obj2 = {
    value: 100,
      foo() {
      console.log(&quot;foo's this: &quot;, this) // {value: 100, foo: f}
      // 메서드 내에서 정의한 중첩함수
      function bar() {
        console.log(&quot;bar's this: &quot;, this) // window
      }
      bar();
    }
};
obj.foo();

// 중첩 함수의 역할은 헬퍼 함수인데 외부 함수인 메서드와 this에 바인딩된 값이 다르면 안된다 -&gt;

// 1. 변수를 만들어 참조하기
const obj3 = {
    value: 100,
      foo() {
      const that = this;
      function bar() {
        console.log(&quot;bar's this.value: &quot;, that.value) // 100
      }
      bar();
    }
};
obj.foo();

// 2. Function.prototype.apply/call/bind 메서드 사용
const obj4 = {
  value: 100,
  foo() {
    function bar() {
      console.log(&quot;bar's this.value: &quot;, this.value);
    }
    bar.bind(this)(); // bar에 .bind(this)를 적용한 후 실행
  }
};
obj4.foo();

// 3. 화살표 함수 사용
const obj5 = {
  value: 100,
  foo() {
    const bar = () =&gt; {
      console.log(&quot;bar's this.value: &quot;, this.value);
    };
    bar();
  }
};
obj4.foo();</code></pre>
<h3 id="✔️-functionprototypeapplycallbind">✔️ Function.prototype.apply/call/bind</h3>
<p><code>apply</code>, <code>call</code>, <code>bind</code>는 Function.prototype의 메서드이므로 모든 함수가 상속받아 사용할 수 있습니다.</p>
<p><code>apply</code>와 <code>call</code>의 본질적인 기능은 함수를 호출하는 것입니다. 또한  첫번째 인자로 this로 사용할 객체를 받아 바인딩합니다.
두번째 인자로는 함수에 전달할 인자 리스트를 받습니다. 여기서 차이가 있는데 <code>apply</code>는 인수를 배열로 묶어 전달하고, <code>call</code>은 인수를 쉼표로 구분해 전달합니다.</p>
<pre><code class="language-js">function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { value: 1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {value: 1}

console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {value: 1}</code></pre>
<p><code>bind</code>는 함수를 호출하지 않고 this로 사용할 객체만 전달합니다.</p>
<pre><code class="language-js">function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { value: 1 };

console.log(getThisBinding.bind(thisArg)());
// {value: 1}</code></pre>


<blockquote>
<p>참고자료
<a href="https://velog.io/@kimlj0814/JS-%EB%B3%80%EC%88%98-%EC%83%9D%EC%84%B1-%EA%B3%BC%EC%A0%95-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85#:~:text=%EC%B0%B8%EA%B3%A0%EC%9E%90%EB%A3%8C-,%EB%AA%A8%EB%8D%98%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%20Deep%20Dive,-%ED%81%B4%EB%A6%B0%EC%BD%94%EB%93%9C%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8">모던 자바스크립트 Deep Dive</a></p>
</blockquote>
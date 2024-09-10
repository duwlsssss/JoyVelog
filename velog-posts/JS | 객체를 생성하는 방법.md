<p>자바스크립트에서 원시 타입을 제외한 모든 데이터 타입(객체, 함수, 배열, 정규표현식 등)은 객체다.</p>
<p>js에서 객체를 생성하는 방법을 알아보자.</p>
<h3 id="1️⃣-object-생성자-함수">1️⃣ Object() 생성자 함수</h3>
<p>자바스크립트에 내장돼 있는 Object 생성자 함수를 이용하는 방법</p>
<pre><code class="language-jsx">const person = new Object(); // 빈 객체 생성

// 빈 객체에 동적으로 프로퍼티 추가해 사용
person.name = 'dodam'; 
person.age = 10;
person.sayHello= function(){
  console.log(`Hello I'm ${this.name}`)
}

console.log(person); // { name: 'dodam', age: 10, sayHello: [Function (anonymous)] }</code></pre>
<h3 id="2️⃣-객체-리터럴">2️⃣ 객체 리터럴</h3>
<p>프로퍼티들이 모여있어 Object() 객체 생성자를 사용하는 것보다 직관적임 </p>
<pre><code class="language-jsx">const person = {
    name : 'dodam',
    age : 10,
    gender: 'male',
    sayHello(){
        console.log(`Hello I'm ${this.name}`)
    }
}</code></pre>
<p>객체의 구조가 동일해도 매번 같은 프로퍼티 작성해야 한다는 단점 있음</p>
<pre><code class="language-jsx">const person1 = {
    name: &quot;kim&quot;,
    sayHello(){
          console.log(`Hello I'm ${this.name}`)
    }
};

console.log(person1.getPersonName()); // Hi, My Name is kim

const person2 = {
    name: &quot;lee&quot;,
    sayHello(){
          console.log(`Hello I'm ${this.name}`)
    }
};

console.log(person2.getPersonName()); // Hi, My Name is lee</code></pre>
<h3 id="3️⃣-생성자-함수">3️⃣ 생성자 함수</h3>
<p>하나의 생성자 함수를 만들어 놓고 이를 템플릿처럼 사용해 구조가 동일한 여러 객체를 만들 수 있음 
함수를 정의해놓고 new 연산자와 함께 호출해 <a href="https://velog.io/@kimlj0814/JS-%EC%83%9D%EC%84%B1%EC%9E%90-%ED%95%A8%EC%88%98">생성자 함수</a>로 호출하는 거임 </p>
<p>인스턴스를 만들때마다 프로퍼티(변수,메서드)를 넣을 공간이 메모리에 만들어짐.
인스턴스들이 공통으로 사용하는 메서드를 생성자 함수 내부에 선언하면, 
인스턴스를 생성할때마다 메모리에 해당 메서드를 넣을 공간이 만들어져 비효율적 
-&gt; Person.prototype.sayHello처럼 prototype에 선언하면, 모든 인스턴스가 해당 메서드를 공유하게 돼 메모리 효율 높임</p>
<pre><code class="language-jsx">const Person = function(name) {
  this.name = name, // 인스턴스별로 고유한 속성(name)은 생성자 함수 내부에서 선언
  Person.prototype.sayHello = function() { // 공통 메서드는 prototype에 선언
    console.log(`Hello I'm ${this.name}`);
  }
}

const dodam = new Person('dodam'); 
const joy = new Person('joy'); 
const hodu = new Person('hodu');

hodu.sayHello(); // 'Hello I'm hodu'</code></pre>
<h3 id="4️⃣-클래스-es6">4️⃣ 클래스 (ES6)</h3>
<p>프로토타입 기반 재사용성, 상속(extends) 등 강력한 기능 가짐</p>
<pre><code class="language-jsx">class Person {
  constructor(name, age) { 
    this.name = name; 
    this.age = age;
  }

  // 메서드는 prototype에 할당되어 모든 인스턴스가 공유함
  sayHello() {
    console.log(`Hello I'm ${this.name}`);
  }

  get personName() { 
  return this.name;
}
  set probName(value) { 
    this.name = value; 
  }
}

const dodam = new Person('dodam', 10);
console.log(dodam.name); // 'dodam'
console.log(dodam.age); // 10
dodam.probName='kim';
console.log(dodam.personName); // 'kim'
dodam.sayHello();
const joy = new Person('joy', 15);</code></pre>
<p>하나의 객체만 사용할 경우, 객체 리터럴 방식이 간단하고 적절할 수 있습니다.
여러 개의 객체를 생성해야 하는 경우에는 생성자 함수나 class 방식이 메모리 효율 면에서 더 유리합니다. prototype을 통해 공통 메서드를 공유하기 때문에, 많은 객체를 만들어도 메모리 사용량을 줄일 수 있습니다.</p>
<blockquote>
<p>참고 자료</p>
</blockquote>
<ul>
<li><a href="https://m.yes24.com/Goods/Detail/92742567">모던 자바스크립트 Deep Dive</a></li>
<li><a href="https://leehwarang.github.io/docs/tech/constructor.html">https://leehwarang.github.io/docs/tech/constructor.html</a></li>
</ul>
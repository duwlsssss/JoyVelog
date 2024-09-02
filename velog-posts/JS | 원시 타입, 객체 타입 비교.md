<h3 id="✅-원시타입불변성-데이터의-신뢰성-보장">✅ 원시타입(불변성, 데이터의 신뢰성 보장)</h3>
<ul>
<li><p><strong>변경 불가한 값</strong>immutable value _원시값이 변하지 않는다는 뜻, 변수는 재할당으로 값 변경 가능(이 때 완전히 새로운 메모리 주소 값이 만들어져 데이터의 신뢰성 보장됨)
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/d14ee097-c830-4f5a-a421-67062766c7ba/image.png" />
변수에 원시값을 재할당하면 새로운 메모리 공간 확보, 그 공간에 재할당한 원시값 저장, 변수가 참조하는 메모리 공간을 변경 
primitive value는 불변성immutability을 가지고 이 값을 할당한 변수는 재할당으로만 값을 변경 가능 </p>
</li>
<li><p><strong>메모리에 실제 값 저장</strong></p>
</li>
<li><p>*<em>원시 값이 복사되어 전달(값에 의한 전달pass by value) *</em>
사실 값을 전달하는 게 아니라 메모리 주소를 전달하는 것임</p>
<pre><code class="language-jsx">let score = 80;
let copy = score; // copy에는 score의 원시값이 복사돼 전달됨 
</code></pre>
</li>
</ul>
<p>console.log(score, copy); // 80 80
console.log(score===copy); // true</p>
<p>// score, copy는 다른 메모리 공간에 저장된 별개의 값임 
// score의 값을 변경해도 copy 값에는 아무 영향도 없음
score = 100;
console.log(score,copy); // 100 80
console.log(score===copy); // false</p>
<pre><code>![](https://velog.velcdn.com/images/kimlj0814/post/caeb20f1-6683-4c8d-883a-c70bf3776ac5/image.png)
![](https://velog.velcdn.com/images/kimlj0814/post/16ba288a-9a4a-401f-af6f-e967f3d16d55/image.png)

+파이썬은 변수에 원시값을 갖는 변수를 할당하는 시점에는 두 변수가 같은 원시값을 참조하다가(같은 메모리 공간) 어느 하나의 변수에 재할당이 일어나면 새로운 메모리공간을 참조하게 동작함 
![](https://velog.velcdn.com/images/kimlj0814/post/0d4c1d86-a986-4727-9fd2-49c5cbdb4cfe/image.png)
위에 있는 코드로 생각하면 score가 메모리 주소를 그대로 전달해 할당 시점에는 score, copy는 같은 메모리 공간에 저장된 같은 값이라는 뜻 
⭐ 중요한 건 변수 할당 시점이든, 두 변수 중 하나의 변수에 재할당이 일어나는 시점이든 결국 **두 변수의 원시값**은 **서로 다른 메모리 공간에 저장된 별개의 값**이 돼 **어느 한쪽에서 재할당을 통해 값을 변경해도 서로 간섭 못 함**

&lt;/br&gt;

### ✅ 객체타입(프로퍼티 접근을 위해 히든클래스 방식)
+js에서 객체
c++, java같은 클래스 기반 객체지향 언어는 사전에 정의된 클래스로 객체(인스턴스)를 생성하고, 객체 생성 이후에 프로퍼티 삭제, 추가 불가능
js에서는 클래스 없이 객체 생성 가능하고, 동적으로 프로퍼티를 조작할 수 있음. 사용은 편하나 클래스 기반 OOP언어의 객체보다 생성 및 프로퍼티 접근 비용이 많이 듦
-&gt; V8 자바스크립트 엔진은 프로퍼티에 접근하기 위해 동적 탐색이 아닌 **hidden class 방식**을 사용해 c++ 객체 프로퍼티에 접근하는 정도의 성능을 보장함

- **변경 가능한 값**mutable value 
객체값은 원시값처럼 크기가 일정하지도 않고, 크기가 매우 클 수도 있음, deep copy해서 생성하는 비용이 많이 듦, 메모리를 효율적으로 사용하기 위해, 객체를 복사해 생성하는 비용을 줄이기 위해 객체는 변경 가능한 값으로 설계돼 있음 -&gt; 원시값과 달리 여러 식별자가 하나의 객체 공유 가능

- **메모리에 참조 값 저장**
변수에 객체를 저장하면 ```객체가 실제로 들어있는 메모리 공간의 주소```(참조값)이 저장됨 
변수는 참조값을 통해 객체에 접근함 

- **원본의 참조값이 복사돼 전달(참조에 의한 전달pass by reference)**
사실 객체가 실제로 들어있는 메모리 주소가 들어있는 메모리 주소가 저장됨
```jsx
const person = {
    name: 'Lee'
}

const copy = person; // copy에는 person의 참조값이 복사돼 전달됨 

// copy, person은 다른 메모리 주소를 갖지만 그 주소 안에 들어있는 값이 같음
console.log(person, copy); // { name: 'Lee' } { name: 'Lee' }
console.log(person===copy); // true

// 둘 중 하나가 객체를 변경(프로퍼티 값 변경, 추가, 삭제)하면 서로 영향을 받음 
person.name=&quot;Kim&quot;;
person.address=&quot;Seoul&quot; // { name: 'Kim', address: 'Seoul' } { name: 'Kim', address: 'Seoul' }
console.log(person, copy); //true
</code></pre><p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/41d520be-14bf-47c1-b312-e59c8a6a49bc/image.png" />
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/30b7e163-ecc7-43a4-9768-21cc36a64362/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/8e8cd143-cca2-4a6a-b277-41adebcb1dcc/image.png" /></p>
<blockquote>
<p>결국 메모리에 저장된 값을 복사해서 전달한다는 면에선 동일(원시 값 or 참조 값)</p>
</blockquote>


<h3 id="✅-얕은-복사-vs-깊은-복사">✅ 얕은 복사 vs 깊은 복사</h3>
<p>✔ 얕은 복사 (Shallow Copy)
객체에 중첩돼 있는 객체의 경우 그 객체 자체가 아닌 참조값을 복사_1단계까지만 복사
JavaScript에서 객체 복사는 기본적으로 얕은 복사로 이뤄짐
원본 객체와 복사된 객체는 중첩된 객체(내부 객체)를 공유하게 됨
얕은복사 방법: object.assign, 전개연산자</p>
<ul>
<li>Spread Operator (<code>...</code>)
ES6부터 추가됨 </li>
</ul>
<pre><code class="language-jsx">const original = {
  a: 1,
  b: { c: 2 }
};

const shallowCopy = { ...original }; // shallowCopy는 original 객체의 얕은 복사본

console.log(original===shallowCopy) // false
console.log(original.b.===shallowCopy.b) // true    중첩됨 객체는 참조값을 복사해서 두 식별자가 하나를 가리키고 있는 것임
shallowCopy.b.c = 3; //원본과 복사본이 중첩된 객체(`b`)를 공유하고, shallowCopy.b.c를 변경하면 original.b.c 도 변경됨 

console.log(original.b.c); // 3</code></pre>
<p>✔ 깊은 복사 (Deep Copy)
객체에 중첩돼 있는 객체까지 모두 복사_내부에 있는 모든 값들을 참조 값이 아닌 완전히 복사
원본 객체와과 복사된 객체는 완전히 독립적임
깊은복사 방법: 재귀함수, lodash</p>
<pre><code class="language-jsx">const original = {
  a: 1,
  b: { c: 2 }
};

// lodash의 cloneDeep 을 사용한 깊은 복사
const_ = require('lodash');
const deepCopy = _.cloneDeep(original); // deepCopy는 original 객체의 깊은 복사본

console.log(original===deepCopy) // false
console.log(original.b.===deepCopy.b) // false    두 식별자는 완전히 독립된 객체를 가리킴 
shallowCopy.b.c = 3; 

console.log(original.b.c); // 2</code></pre>


<h3 id="✅-함수의-호출-방식">✅ 함수의 호출 방식</h3>
<p>call by value: 값에 의한 호출(함수 안에서 인자의 값이 변경되어도 외부 변수의 값은 변하지 않음)</p>
<p>원시 값을 복사해서 전달
call by reference: 참조에의한 호출(함수안에서 인자의 값이 변경되면 아규먼트로 전달된 객체의 값이 변경)</p>
<p>참조 값을 전달 → 안에 값 바뀌면 밖의 값도 바뀜
call by reference, call by value in JS</p>
<p>C와 C++과 같은 포인터가 있는 언어에서는 이 개념이 그대로 적용된다고 생각하지만 JS의 경우 값에 의한 전달(call by sharing이라고도 하지만 공식적인건 X)만 적용되어 실제 객체를 파라미터로 넘겼을 때 참조값에 존재하는 동일한 값이 복사되어 전달되기 때문이다.</p>
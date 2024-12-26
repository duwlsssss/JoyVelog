<p>알고리즘 문제를 풀다 수의 범위에 대한 이해가 부족함을 느꼈고 
Number와 BigInt의 개념을 정리해보려 합니다.</p>
<br />

<p>제가 풀었던 <a href="https://www.acmicpc.net/problem/13305">문제</a>에서 </p>
<pre><code>도시의 개수를 나타내는 정수 N(2 ≤ N ≤ 100,000)가 주어진다.
제일 왼쪽 도시부터 제일 오른쪽 도시까지의 거리는 1이상 1,000,000,000 이하의 자연수이다. 
리터당 가격은 1 이상 1,000,000,000 이하의 자연수이다. </code></pre><p>라는 입력이 주어졌습니다.
도시간 거리를 이동할 기름을 계산해야 해 10^18의 범위까지 반영해야했습니다.
처음엔 Number를 썼지만</p>
<pre><code class="language-js">const fs = require('fs')
const filePath = process.platform === 'linux' ? '/dev/stdin' : './example.txt'
const input = fs.readFileSync(filePath).toString().trim().split('\n')

const N = Number(input[0])
const distances = input[1].split(' ').map(Number) // 도시간 거리 배열
const prices = input[2].split(' ').map(Number) // 도시의 리터탕 기름값 배열

let answer = 0;
let currentPrice = prices[0];

for (let i = 0; i &lt; N - 1; i++) {
  answer += currentPrice * distances[i];

  if (prices[i + 1] &lt; currentPrice) {
    currentPrice = prices[i + 1];
  }
}

console.log(answer)</code></pre>
<p>모든 케이스를 반영하지 못했습니다.</p>
<pre><code class="language-js">const distances = input[1].split(' ').map(BigInt)
const prices = input[2].split(' ').map(BigInt)

let answer = 0n;

console.log(String(answer))</code></pre>
<p>이렇게 BigInt를 사용함으로써 더 큰 수도 반영할 수 있게 됐습니다. </p>
<p>어떻게 이렇게 해결한 것인지 알아봅시다. </p>
<br />

<h3 id="number-데이터-타입">Number 데이터 타입</h3>
<p><code>Number</code>는 JavaScript의 기본 숫자 타입입니다. (모든 숫자는 기본적으로 Number로 저장됨)
Number는 <a href="https://en.wikipedia.org/wiki/Floating-point_arithmetic">부동 소수점 값</a>입니다. 이는 정밀도가 제한적이라는 것을 의미하고 따라서 number 타입만으로는 모든 수를 다 표현할 수 없습니다.</p>
<p>Number 객체의 MAX_SAFE_INTEGER, MIN_SAFE_INTEGER 프로퍼티로 안전한 최대, 최소 정수를 알 수 있습니다.</p>
<pre><code class="language-js">Number.MAX_SAFE_INTEGER // js에서 안전한 최대 정수. (2^53 - 1)  =&gt;  9007199254740991

Number.MIN_SAFE_INTEGER // js에서 안전한 최소 정수. (-(2^53 - 1)) =&gt; -9007199254740991</code></pre>
<p>여기서 <code>안전함</code>이라는 말은 정수를 정확히 표현할 수 있다는 뜻입니다.</p>
<p>Number 데이터 타입을 통해 계산한 결과가 -(2^53 - 1) ~ (2^53 - 1) 까지는 안전한 결과 값을 계산합니다.</p>
<p>MAX_SAFE_INTEGER를 초과하거나 MIN_SAFE_INTEGER보다 작은 결과값으로 평가되는 표현식은 에러는 발생시키지 않지만 정상적인 값을 출력하지 않는 것입니다.</p>
<pre><code class="language-js">let max = Number.MAX_SAFE_INTEGER;

console.log(max); // 9007199254740991
console.log(max++); // 9007199254740991</code></pre>
<p>➕ 현재 Number 데이터 타입의 값이 안전한 값인지 아닌지 확인하기 위해 Number.isSafeInteger() 메서드를 통해 확인할 수 있습니다.</p>
<pre><code class="language-js">let max = Number.MAX_SAFE_INTEGER;

console.log(Number.isSafeInteger(max));  // true
console.log(Number.isSafeInteger(++max)); // false</code></pre>
<br />

<p>그럼 -(2^53 - 1) ~ (2^53 - 1)를 넘어가는 수는 어떻게 표현할까요?</p>
<h3 id="bigint-데이터-타입">BigInt 데이터 타입</h3>
<p><code>BigInt</code>는 Number가 안정적으로 나타낼 수 있는 최대치인 2^53 - 1보다 큰 정수를 표현할 수 있는 js 내장 객체입니다.</p>
<p>BigInt를 사용하면 숫자에 대한 안전한 정수 제한을 초과하여 큰 정수를 안전하게 저장하고 조작 할 수 있습니다.</p>
<h4 id="bigint-사용법">BigInt 사용법</h4>
<ul>
<li>정수 리터럴 뒤에 n을 붙이기<pre><code class="language-js">let x = 1234567890123456789 * 123;
console.log(x); // 151851850485185200000 ❌
</code></pre>
</li>
</ul>
<p>x = 1234567890123456789n * 123n;
console.log(x); // 151851850485185185047n 🟢</p>
<pre><code>
- BigInt() 생성자 함수 호출
```js
x = BigInt(x);
console.log(typeof x); // bigint</code></pre><h4 id="bigint-특징">BigInt 특징</h4>
<ul>
<li>BigInt는 Number와 일치하지 않지만 동등합니다<pre><code class="language-js">0n === 0 // false
</code></pre>
</li>
</ul>
<p>0n == 0 // true</p>
<p>1n &lt; 2 // true</p>
<pre><code>
- if, &amp;&amp;, || 또는 Boolean()을 사용할 때, BigInt는 Number와 같은 논리를 따릅니다 (0n은 falsy 값)
```js
if (0n) {
  console.log('if에서 안녕!');
} else {
  console.log('else에서 안녕!');
}
// "else에서 안녕!"

0n || 12n // 12n

0n &amp;&amp; 12n // 0n

Boolean(0n) // false</code></pre><ul>
<li>사용 가능 연산자 : +, <em>, -, *</em>, %<pre><code class="language-js">const previousMaxSafe = BigInt(Number.MAX_SAFE_INTEGER);
// ↪ 9007199254740991
</code></pre>
</li>
</ul>
<p>const maxPlusOne = previousMaxSafe + 1n;
// ↪ 9007199254740992n</p>
<p>const multi = previousMaxSafe * 2n;
// ↪ 18014398509481982n</p>
<p>const subtr = multi – 10n;
// ↪ 18014398509481972n</p>
<p>const mod = multi % 10n;
// ↪ 2n</p>
<p>const bigN = 2n ** 54n;
// ↪ 18014398509481984n</p>
<pre><code>❗ 소수점 결과를 포함하는 연산을 BigInt와 사용하면 소수점 이하는 사라집니다
```js
const rounded = 5n / 2n;
// ↪ 2.5n이 아니라 2n</code></pre><p>❗ BigInt는 내장 Math 객체의 메서드와 함께 사용할 수 없습니다</p>
<pre><code class="language-js">const x = BigInt(100);
const y = BigInt(200);

console.log(Math.max(x, y)); // ❌ TypeError: Cannot convert a BigInt value to a number at Math.max (&lt;anonymous&gt;)</code></pre>
<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number</a></li>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt</a></li>
</ul>
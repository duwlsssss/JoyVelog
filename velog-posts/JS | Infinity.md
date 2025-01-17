<p>백준 3097 <a href="https://www.acmicpc.net/status?user_id=rladuwls0814&amp;problem_id=3097&amp;from_mine=1">산책 경로</a>
문제를 풀며 Infinity의 필요성에 대해 배웠습니다.</p>
<p>이 문제에선 주어진 좌표의 선분을 하나 제거했을때 나올 수 있는 최소 거리를 구하는 게 핵심이었어요.</p>
<p>주어진 좌표를 배열로 만들고 시작 좌표에 더해 최종 위치를 계산했어요.
그 다음 최종 위치에서 배열의 각 테스트케이스 인덱스의 좌표를 빼서 선분 하나를 제거했을 때의 거리를 구했어요. 
그 거리를 비교하며 최소 거리를 업데이트 했는데요. 이 부분에서 문제가 생겼어요.</p>
<pre><code class="language-js">// 풀이 코드
const fs = require('fs');
const filePath = process.platform === 'linux' ? '/dev/stdin' : './example.txt';
const input = fs.readFileSync(filePath).toString().trim().split('\n');

const N = Number(input[0]);
const posArr = Array.from(Array(N), (_, i) =&gt; input[i + 1].split(' ').map(Number));

const solution = () =&gt; {
  let startPos = [0, 0];

  for (const [dx, dy] of posArr) {
    startPos[0] += dx;
    startPos[1] += dy;
  }

  let minDistance = Infinity;

  for (let i = 0; i &lt; N; i++) {
    let newX = startPos[0];
    let newY = startPos[1];

    const [dx, dy] = posArr[i];
    newX -= dx;
    newY -= dy;

    const distance = Math.sqrt(newX ** 2 + newY ** 2);
    minDistance = Math.min(minDistance, distance);
  }
  console.log(startPos[0], startPos[1]);
  console.log(minDistance.toFixed(2));
};

solution();</code></pre>
<p>처음에는 
최소 거리를 구하기 전 minDistance(최소거리 변수)를 이렇게 초기화했어요.</p>
<pre><code>let minDistance = Math.sqrt(startPos[0] ** 2 + startPos[1] ** 2);  // startPos는 모든 선분 반영한 최종 위치</code></pre><p>그런데 계속 틀렸다고 나오더라고요...</p>
<p>문제를 자세히 보면 선분을 제거하지 않았을때 최종 거리는 두 번째 줄에 출력되면 안 돼요.</p>
<p><code>둘째 줄에는 선분을 하나 제거했을 때, 산책을 마치는 위치와 시작한 위치 사이 거리의 최솟값을 반올림해 소수점 둘째 자리까지 출력한다.</code></p>
<p>즉 처음 할당해 준 값이 최솟값이 되는 상황이 생길 수 있는데 그러면 틀린거죠.</p>
<p>이 경우처럼 기준이 달라 논리적 충돌이 생길 수 있는 상황에선 
비교하며 최솟값을 구할때 Infinity를 사용해야 한다고 해요. 
어떤 값과 비교하더라도 초기값이 항상 갱신되기 때문이죠.
추가로 최댓값을 구해야 한다면 
초기값을 -Infinity로 설정하는 것이 안전하다고 해요.</p>
<pre><code>let minDistance = Infinity;</code></pre><br />

<h2 id="💡-infinity">💡 Infinity</h2>
<p>Infinity는 양의 무한대를 나타내는 숫자 값인 전역 객체의 속성이에요.
<code>Number.POSITIVE_INFINITY</code>와 동일한 값이라고 해요.</p>
<h3 id="특징-">특징 :</h3>
<p>Infinity는 JavaScript에서 가장 큰 수로 취급돼요. 하지만 수학적인 무한대와는 약간 달라요.
수학에선 무한대가 정확한 값이 아닌 극한 개념이라면 Infinity는 유한한 수의 범위 내에서 연산이 정의된 수에요.</p>
<ul>
<li><p>어떤 숫자와 Infinity를 더하거나 곱할 때, 결과는 Infinity로 나와요</p>
<ul>
<li>Infinity,양수와 Infinity를 곱한 결과는 Infinity</li>
<li>-Infinity,음수와 Infinity를 곱한 결과는 -Infinity</li>
<li>양수를 Infinity로 나눈 결과는 0</li>
<li>음수를 Infinity로 나눈 결과는 -0</li>
<li>Infinity를, -Infinity를 제외한 음수로 나눈 결과는 -Infinity</li>
<li>Infinity를, Infinity를 제외한 양수로 나눈 결과는 -Infinity</li>
</ul>
</li>
<li><p>NaN과의 관계</p>
<ul>
<li>0을 Infinity로 나눈 결과는 NaN</li>
<li>NaN에 Infinity를 곱한 결과는 NaN</li>
<li>Infinity를 -Infinity 또는 Infinity로 나눈 결과는 NaN</li>
</ul>
</li>
</ul>
<pre><code class="language-js">console.log(Infinity); /* Infinity */
console.log(Infinity + 1); /* Infinity */
console.log(Math.pow(10, 1000)); /* Infinity */
console.log(Math.log(0)); /* -Infinity */
console.log(1 / Infinity); /* 0 */
console.log(1 / 0); /* Infinity */
console.log(isFinite(Infinity)) /* false */</code></pre>
<br />

<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Infinity">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Infinity</a></li>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY</a></li>
</ul>
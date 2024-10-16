<p><strong>Map</strong>과 <strong>Set</strong>은 기존 배열과 객체의 한계를 극복하기 위해 도입된 자료구조</p>
<h3 id="map">Map</h3>
<p>: 해시 테이블을 기반으로 한 자료구조로 키가 있는 데이터를 저장한다는 점에서 객체와 비슷함, 
반면 키에 다양한 자료형 허용(객체처럼 키를 string으로 변환하지 않음), 키의 삽입 순서를 기억함, 이터러블 객체임, 하나의 Map에서 키는 유일한 값임</p>
<ul>
<li><p>주요 메소드</p>
<pre><code class="language-jsx">const map = new Map([iterable]) // Map 생성. 키값쌍이 있는 iterable 객체를 선택적으로 넘겨 맵 초기화에 사용 가능
map.set(key, value) // key를 이용해 value를 저장 후 map 자신 반환-&gt;메서드 체이닝 가능
map.get(key) // key에 해당하는 값을 반환. key가 존재하지 않으면 undefined를 반환. O(1)-해시테이블을 사용해 상수시간에 처리됨
map.has(key) // key가 존재하면 true, 존재하지 않으면 false를 반환 O(1)
map.delete(key) // key에 해당하는 값을 삭제. 성공시 true 실패시 false 반환 O(1)
map.clear() // 맵 안의 모든 요소를 제거, undefined 반환 
map.size // 요소의 개수 반환
map.keys() // 키를 반복 가능한(iterable, 이터러블) 객체로 반환
map.values() // 값을 이터러블 객체로 반환
map.entries() // [키, 값]쌍을 가진 이터러블 객체를 반환
map.forEach(callbackFn,[, tisArg]) // 키-값 쌍에 대해 삽입 순서대로 callbackFn을 한 번씩 호출,
//thisArg 매개변수가 있다면 각 콜백의 this 값으로 사용됨, undefined 반환 O(N)</code></pre>
</li>
<li><p>특징</p>
<ul>
<li><p>Map에서는 SameValueZero 알고리즘으로 (NaN과 NaN을 같다고 봄, 나머지 값은 === 연산자와 유사) 값의 일치 여부 확인함 -&gt; 키를 비교하는 방식임</p>
<ul>
<li>키로 다양한 자료형 허용<pre><code class="language-jsx">// 💡 객체를 키로 사용 가능
// 고객의 가게 방문 횟수
let visitsCountMap = new Map();
</code></pre>
</li>
</ul>
<p>let john = { name: "John" };</p>
<p>visitsCountMap.set(john, 123);</p>
<p>console.log( visitsCountMap.get(john) ); // 123 출력</p>
<p>// 객체를 사용했다면?
let visitsCountObj = {};
visitsCountObj[john] = 123; // 객체는 모든 키를 string type으로 변환시킴. 이 과정에서 john은 문자형으로 변환되어 "[object Object]"가 됨
console.log( visitsCountObj["[object Object]"] ); // 123 출력</p>
<p>// 💡 NaN을 키로 사용 가능
// Map은 NaN과 NAN을 같다고 판단한다고 했음
const myMap = new Map();
myMap.set(NaN, "not a number");</p>
<p>myMap.get(NaN); // "not a number"</p>
<p>const otherNaN = Number("foo");
myMap.get(otherNaN); // "not a number"</p>
<pre><code>- `for...of`, `forEach()` 로 순회 가능 
맵은 이터러블 객체이고 키-값쌍이 삽입된 순서를 기억하므로 키의 삽입 순서대로 순회 가능
```jsx
const myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");

// [키,값] 쌍을 대상으로 순회
for (const [key, value] of myMap) {
console.log(`${key} = ${value}`);
}
// 0 = zero 출력
// 1 = one 출력

// [키,값] 쌍을 대상으로 순회
for (const [key, value] of myMap.entries()) {
console.log(`${key} = ${value}`);
}
// 0 = zero 출력
// 1 = one 출력

// Map은 entries() 메소드와 동일하게 키-값쌍의 이터레이터를 반환하도록 구현되어 있음
// entries()를 명시적으로 호출하는 방식은 조금 더 명확하게 의도를 드러내지만, 결과는 같음

// 키를 대상으로 순회
for (const key of myMap.keys()) {
console.log(key);
}
// 0 출력
// 1 출력

// 값을 대상으로 순회
for (const value of myMap.values()) {
console.log(value);
}
// zero 출력
// one 출력

// 배열과 유사하게 내장 메서드 forEach도 지원함
myMap.forEach((value, key, myMap) =&gt; {
console.log(`${key} = ${value}`);
});
// 0 = zero 출력
// 1 = one 출력</code></pre><ul>
<li>객체, 배열(키,값쌍을 가진 2차원 배열)로 Map 생성 / Map으로 객체, 배열 생성 가능 <pre><code class="language-jsx">const kvArr = [
["key1", "value1"],
["key2", "value2"],
];
const obj= {
key1: "value1",
key2: "value2",
};
</code></pre>
</li>
</ul>
<p>// 키,값쌍을 가진 2차원 배열을 Map으로 만듦
const mapFromArr = new Map(kvArr);
console.log(mapFromArr.get("key1")); // value1 출력</p>
<p>// 객체를 Map으로 만듦
const mapFromObj = new Map(Object.entries(obj)); // Object.entries로 객체 obj를 배열 [["name","John"], ["age", 30]]로 바꾸고, 이 배열로 새로운 맵을 만듦
console.log(mapFromObj.get("key1")); // value1 출력</p>
<p>console.log(mapFromArr===mapFromObj); // false - 내용은 같지만 메모리 주소(참조값)가 다름</p>
<p>// Map을 배열로 만듦 
console.log(Array.from(mapFromArr)); 
console.log([...mapFromArr]);
// [ [ 'key1', 'value1' ], [ 'key2', 'value2' ] ] - kvArray와 같은 내용 출력</p>
<p>// Map을 객체로 만듦
console.log(Object.fromEntries(mapFromObj)); // Object.fromEntries는 이터러블 객체를 입력으로 받아 그 키-값 쌍을 객체로 변환해줌
// { key1: 'value1', key2: 'value2' } 출력</p>
<p>// 또는 키만, 값만 따로 배열로 만들 수 있음
console.log(Array.from(mapFromArr.keys())); // ["key1", "key2"] 출력</p>
<pre><code>- new Map()에 Map을 인자로 넣어 복사 가능
```jsx
const original = new Map([[1, "one"]]);

const clone = new Map(original); // 얕은 복사 일어남 

console.log(clone.get(1)); // one 출력
console.log(original === clone); // false 출력</code></pre><ul>
<li>키값쌍을 반환하는 이터러블 객체라면 new Map()의 인자로 넣어 Map 생성 가능</li>
</ul>
<pre><code class="language-jsx">const mySet = new Set([
["key1", "value1"],
["key2", "value2"]
]);
const mapFromSet = new Map(mySet);</code></pre>
<ul>
<li>키 유일성을 유지한채로 병합 가능<pre><code class="language-jsx">const first = new Map([
[1, "one"],
[2, "two"],
[3, "three"],
]);
</code></pre>
</li>
</ul>
<p>const second = new Map([
[1, "uno"],
[2, "dos"],
]);</p>
<p>// 두 맵을 병합할 때 키 값이 중복될 경우 마지막 키의 값을 따름
const merged = new Map([...first, ...second, [1, "eins"]]);</p>
<p>console.log(merged.get(1)); // eins 출력
console.log(merged.get(2)); // dos 출력
console.log(merged.get(3)); // three 출력</p>
<pre><code></code></pre></li>
</ul>
</li>
</ul>
<br />

<h3 id="set">Set</h3>
<p>: 중복을 허용하지 않는 집합 자료구조. 해시 테이블을 기반으로 함</p>
<ul>
<li><p>주요 메소드</p>
<pre><code class="language-jsx">const set = new Set([iterable]) // 셋을 만듦. 이터러블 객체를 전달받으면(보통 배열) 그걸로 셋 초기화 함
set.add(value) // 값을 추가 후 셋 자신을 반환 O(1) 
set.delete(value) // 값을 제거. 성공하면 true, 아니면 false를 반환 O(1) 
set.has(value) // 셋 내에 값이 존재하면 true, 아니면 false를 반환 O(1) 
set.clear() // 셋을 비움, undefined 반환
set.size // 값 개수 반환
set.keys() // 셋 내의 모든 값을 포함하는 이터러블 객체를 반환.
set.values() // set.keys와 동일한 작업을 함. 맵과의 호환성을 위해 만들어짐
set.entries() // 셋 내의 각 값을 이용해 만든 [value, value] 배열을 포함하는 이터러블 객체를 반환. 맵과의 호환성을 위해 만들어짐.
set.forEach(callbackFn,[, tisArg]) // 값에 대해 삽입 순서대로 callbackFn을 한 번씩 호출,
// thisArg 매개변수가 있다면 각 콜백의 this 값으로 사용됨, undefined 반환 O(N)</code></pre>
</li>
<li><p>특징</p>
<ul>
<li><p>집합 연산을 할 수 있는 메소드 제공</p>
<img src="https://velog.velcdn.com/images/kimlj0814/post/9b84c5d1-b7c8-4f99-beac-35b41dfe66b2/image.png" width="50%/" />

<p>위 메소드의 인자에는 유사 Set 객체 쓸 수 있음(size, has(), keys()를 갖는 객체)</p>
<pre><code class="language-jsx">const a = new Set([1, 2, 3]);
const b = new Map([
[1, "one"],
[2, "two"],
[4, "four"],
]);
/*
function union(setA, setB) {
  const _union = new Set(setA);
  for (const elem of setB) {
      _union.add(elem);
    }
    return _union;
  }
  */

// Map 객체는 size, has(), keys()를 가지고 있어 Set의 메서드를 사용했을때 아래처럼 키 Set처럼 동작함
console.log(a.union(b)); // Set(4) {1, 2, 3, 4} 출력 
</code></pre>
</li>
<li><p><code>for...of</code>, <code>forEach()</code> 로 순회 가능
셋은 이터러블 객체이고 키에 삽입된 순서를 기억하므로 값의 삽입 순서대로 순회 가능
```jsx
const set = new Set([1, "text", { "a": 1, "b": 2 }, 5]);</p>
<p>for (const item of set){
console.log(item);
}
for (const item of set.keys()) {
console.log(item);
}
for (const item of set.vlaues()) {
console.log(item);
}
for (const [key, value] of mySet1.entries()) { // key와 value가 같음
console.log(key);
}
/*
1
text
{ a: 1, b: 2 }
5 출력</p>
</li>
<li><p>/</p>
<p>// forEach를 사용해도 동일하게 동작 - 
set.forEach((value, valueAgain, set) =&gt; { // 키가 없으니까 value, vlaueAgain은 같은 값임 
// Map과 비슷하게 만든거고 실제로는 value만 쓰면 됨
  console.log(value);
});</p>
<pre><code>```jsx
//Set와 배열의 관계
const nums = [1, 2, 2, 2, 3, 4]
console.log([...new set(nums)]); // [1, 2, 3, 4] 출력

//Set와 문자열의 관계
new Set("firefox"); // Set(6) [ "f", "i", "r", "e", "o", "x" ]</code></pre></li>
</ul>
</li>
</ul>
<ul>
<li>Object vs Map vs Set</li>
</ul>
<table>
<thead>
<tr>
<th align="left"></th>
<th align="center">Object</th>
<th align="center">Map</th>
<th align="center">Set</th>
</tr>
</thead>
<tbody><tr>
<td align="left">프로토타입 여부</td>
<td align="center"><strong>Object.prototype</strong>을 상속받아 그에 정의된 메서드들과 직접 정의한 키가 충돌할 수 있음</td>
<td align="center">직접 정의한 내용만 가짐</td>
<td align="center">직접 정의한 내용만 가짐</td>
</tr>
<tr>
<td align="left">키 유형</td>
<td align="center">string, symbol만 허용 나머지는 자동으로 문자열로 변환됨</td>
<td align="center">모든 자료형</td>
<td align="center">값만 저장(모든 자료형)</td>
</tr>
<tr>
<td align="left">키 순서</td>
<td align="center">순서가 항상 유지되진 않으니 순서에 의존하지 말자(숫자 키는 자동으로 오름차순 정렬됨)</td>
<td align="center">항상 삽입 순서 유지됨</td>
<td align="center">값의 삽입 순서 항상 유지됨</td>
</tr>
<tr>
<td align="left">순회</td>
<td align="center">for...in으로 키는 직접 순회 가능, 객체에 직접 for...of는 못쓰고 Object.entries(), Object.keys(), Object.values()로 이터러블로 변환한 후 사용 가능</td>
<td align="center">iterable(for...of, Map.prototype.forEach() 사용 가능)</td>
<td align="center">iterable(for...of, Set.prototype.forEach() 사용 가능)</td>
</tr>
<tr>
<td align="left">크기 확인</td>
<td align="center">별도의 코드 필요</td>
<td align="center">map.size() 메소드</td>
<td align="center">set.size() 메소드</td>
</tr>
</tbody></table>
<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://ko.javascript.info/map-set">https://ko.javascript.info/map-set</a></li>
<li><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map</a></li>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set</a></li>
</ul>
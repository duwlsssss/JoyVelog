<p>vanila.js로 스타벅스 페이지 클론을 하고 있었다.</p>
<pre><code class="language-html">&lt;li class=&quot;search&quot;&gt;
  &lt;input type=&quot;text&quot;/&gt;
  &lt;div class=&quot;material-icons search-icon&quot;&gt;search&lt;/div&gt;
&lt;/li&gt;</code></pre>
<pre><code class="language-css">.search{
  ...
  input{
    display: none;
    padding: 0.2rem;
    ...
  }
}</code></pre>
<p>input은 display: none으로 숨기다가 search를 클릭하면 input 필드와 search icon을 나란히 표시하려했다.</p>
<p>js를 아래같이 작성했다.</p>
<pre><code class="language-jsx">const searchEl = document.querySelector('.search');
const searchInputEl = searchEl.querySelector('input');
searchEl.addEventListener('click',function(){
    if (searchInputEl.style.display === 'none') {
      searchInputEl.style.display = 'block'; 
      searchInputEl.focus(); 
    }
});</code></pre>
<p>그런데 아무리 search 영역을 클릭해도 input이 추가되지 않았다...
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/a203819c-cceb-4fb1-8702-bac04318a53f/image.png" /></p>
<p>콘솔에 찍어보니 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/f7496cac-262b-4535-b76d-70b697694064/image.png" />
<code>searchInputEl.style.display</code>가 빈 값이었다.</p>
<br />

<p>인라인 스타일(JavaScript에서 element.style로 설정된 스타일, HTML의 style 속성에 직접 지정된 스타일)은 인라인 스타일만 포함한다.
인라인 스타일로 설정된 값만 js에서 <code>element.style.~</code>을 통해 확인할 수 있는 것이다. 
CSS 파일이나 <code>&lt;style&gt;</code> 태그에서 설정된 스타일은 인라인 스타일이 아니므로 확인할 수 없다.</p>
<p>내 상황에선 input 태그가 <strong>CSS에 의해</strong> display: none 상태이므로 JavaScript에서 element.style.display를 확인해도 값이 나오지 않았다. </p>
<br />

<pre><code class="language-jsx">window.onload = function() {
  searchInputEl.style.display = 'none';
};</code></pre>
<p>이렇게 인라인 스타일을 추가했더니 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/469eaffa-abec-401f-bc9e-b0a72a753ab6/image.png" />
원하는 대로 동작했다.</p>
<p>그럼 css에 지정한 스타일을 js에서 쓰고 싶으면?</p>
<h3 id="windowgetcomputedstyle">window.getComputedStyle()</h3>
<pre><code class="language-jsx">let style = window.getComputedStyle(element[, pseudoElt]);</code></pre>
<p>이 메소드는 인자로 전달받은 요소의 모든 스타일(CSS, 인라인 스타일, 브라우저 기본 스타일)을 결합해 최종적으로 적용된 값을 담은 객체를 반환한다. 
pseudoElt는 optional 인자이고
일치시킬 의사요소(pseudo element)를 지정하는 문자열이다. 보통의 요소들에 대해서는 생략되거나 null이어야 한다.</p>
<p>반환값은 요소의 스타일이 변경 될 때 자동으로 업데이트되는 읽기 전용 CSSStyleDeclaration 객체이다.</p>
<p><code>window.getComputedStyle(searchInputEl).display</code> 이런식으로 js에서 원하는 속성을 쓸 수 있다.</p>
<pre><code class="language-jsx">searchEl.addEventListener('click',function(){
  const computedStyle = window.getComputedStyle(searchInputEl);
  console.log(`computedStyle.display: ${computedStyle.display}`);
  console.log(`computedStyle.getPropertyValue(&quot;display&quot;): ${computedStyle.getPropertyValue(&quot;display&quot;)}`);
  if (computedStyle.display === 'none') {
    searchInputEl.style.display = 'block'; 
    searchInputEl.focus(); 
  }
});</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/33b383a0-ea38-47ee-829e-aab74767e40b/image.png" /></p>
<p>::after, ::before, ::marker, ::line-marker 같은 의사 요소를 두번째 인자로 넣어 사용할 수도 있다.</p>
<pre><code>&lt;style&gt;
  h3::after {
    content: &quot; rocks!&quot;;
  }
&lt;/style&gt;

&lt;h3&gt;generated content&lt;/h3&gt;

&lt;script&gt;
  var h3 = document.querySelector(&quot;h3&quot;);
  var result = getComputedStyle(h3, &quot;:after&quot;).content;

  console.log(&quot;the generated content is: &quot;, result); //' rocks!'
&lt;/script&gt;
</code></pre><br />

<p><strong>정확한 스타일 상태를 확인하고 싶으면 window.getComputedStyle()로 CSS나 인라인 스타일 구분 없이 최종 적용된 스타일을 확인해야한다.</strong></p>
<br />

<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://developer.mozilla.org/ko/docs/Web/API/Window/getComputedStyle">https://developer.mozilla.org/ko/docs/Web/API/Window/getComputedStyle</a></li>
</ul>
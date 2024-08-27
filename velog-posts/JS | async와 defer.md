<p>브라우저는 <code>&lt;script&gt;</code> 태그를 만나면 javaScript파일을 <strong>다운로드, 파싱, 실행</strong>할 때까지 <strong>HTML 파싱을 중단</strong>한다. <code>&lt;body&gt;</code> 태그의 마지막 줄에 <code>&lt;script&gt;</code>를 사용하면 HTML 파싱이 다 완료된 상태이므로 괜찮지만</p>
<p><code>&lt;script&gt;</code>를 <code>&lt;body&gt;</code> 중간에 사용할 경우 HTML 파싱을 하는 도중에 갑자기 js 파일을 읽게 되므로 오류 발생 위험이 있다.</p>
<p>또 html 파싱을 중단하기 때문에 실행해 <strong>렌더링 시간이 늘어난다</strong>.</p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/44ead3e8-23fa-48b3-befc-e912e1f0a420/image.png" /></p>
<p>기본적인 <code>&lt;script&gt;</code>를 사용할 때 렌더링 순서이다. js를 가져와 실행이 끝날때까지 html 파싱을 멈춰 화면이 출력되는 시간이 길어진다.</p>
<p>이 경우 생산성을 높이기 위해 async와 defer 속성을 사용해 <strong>html 파싱과 스크립트 다운로드를 병렬로 처리</strong>할 수 있다.
<br />
<strong>공통점</strong>: 두 속성 모두 자바스크립트 파일을 비동기적으로 다운로드하게 하며 그 동안 HTML 파싱은 중단되지 않는다.</p>
<p><strong>차이점</strong> :
<code>&lt;script async ...&gt;</code> : 자바스크립트 파일이 다운로드가 완료되는 즉시 실행된다.  js가 실행될때 HTML 파싱은 중단된다.
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/189c626d-06b0-49c8-be46-4ab160b10a12/image.png" /></p>
<p>여러 async스크립트가 있으면 다운로드된 순서대로 실행된다.</p>
<pre><code>&lt;script src=&quot;a.js&quot; async&gt;
&lt;script src=&quot;b.js&quot; async&gt;
&lt;script src=&quot;c.js&quot; async&gt;</code></pre><p>a.js를 다운로드하는데 3초, b.js를 다운로드하는데 2초, c.js를 다운로드하는데 1초가 걸린다면, c.js, b.js, a.js 순서로 스크립트가 실행된다.</p>
<p><strong>언제 사용?</strong> : DOM에 종속성이 없는 스크립트, 스크립트 간 실행 순서가 중요하지 않은 경우에 사용</p>
<p><code>&lt;script defer ...&gt;</code>: HTML 파싱이 완료된 후에 다운로드 된 자바스크립트를 실행한다. 여러 defer 스크립트가 있으면 정의된 순서대로 실행된다. 
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/22b59d2e-6ddf-457f-a553-e8990fd473a2/image.png" /></p>
<p>DOMContentLoaded 발생 이전에 실행해야 함을 나타내는 불리언 속성이기도 하다.
defer를 사용하지 않으면 기본적으로 true, 사용하면 false이다.</p>
<p><strong>언제 사용?</strong> : DOM이 모두 생성된 후 실행되야 하는 스크립트, 스크립트 간 실행 순서가 준요한 경우 사용</p>
<br />
참고 페이지

<ul>
<li><a href="https://webroadcast.tistory.com/15">https://webroadcast.tistory.com/15</a></li>
<li><a href="https://web.dev/articles/critical-rendering-path/adding--interactivity-with-javascript?hl=ko#parser_blocking_versus_asynchronous_javascript">https://web.dev/articles/critical-rendering-path/adding--interactivity-with-javascript?hl=ko#parser_blocking_versus_asynchronous_javascript</a></li>
<li><a href="https://beomy.github.io/tech/browser/async-defer/">https://beomy.github.io/tech/browser/async-defer/</a></li>
</ul>
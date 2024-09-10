<h3 id="✅-csr-client-side-rendering">✅ CSR (Client-Side Rendering)</h3>
<p>: 서버가 최소한의 HTML만 클라이언트에 보내고, 클라이언트가 JavaScript를 실행해 추가적인 작업(리액트 코드도 실행)을 거친 후 전체 페이지를 로드함 -&gt; viewable,interactable
빠른 화면 전환이 가능해 인터랙티브한 웹에 적합함
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/33504b26-2fdd-4fd8-9c21-fd241297f38a/image.png" /></p>
<p>SSR (Server-Side Rendering):
서버에서 미리 렌더링된 HTML이 로드되면 페이지는 즉시 보여질 수(viewable) 있지만, 리액트의 hydration 과정이 완료된 후에야 <strong>상호작용(interactable)</strong>이 가능합니다.</p>
<h3 id="✅-ssr-server-side-rendering">✅ SSR (Server-Side Rendering)</h3>
<p>: 서버에서 미리 렌더링 된 완전한 html을 클라이언트에게 보내면(리액트 코드도 미리 실행된 결과) 클라이언트는 이를 로드함 -&gt; viewable 
이후 추가적인 JavaScript 작업(hydration)을 함 (리액트 활성화) -&gt; interactable
seo에 유리(검색엔진이 완성된 html을 쉽게 읽을 수 있음), 페이지를 바꿀 때마다 서버가 html을 생성해야 해 렌더링해야해 서버 부하가 커지고 속도가 느려질 수 있음
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/5aa85b5d-8920-4c82-bbcc-d28f2788596b/image.png" /></p>


<p><strong>데이터 호출</strong>은 CSR과 SSR 모두 클라이언트에서 페이지가 렌더링된 후에도 발생할 수 있음 
but 초기 데이터 로드 방식은 다름. 
CSR에서는 클라이언트가 javascript를 통해 데이터를 동적으로 가져온 후 최종 페이지 렌더링.
SSR에서는 서버가 초기 데이터까지 포함한 완전한 HTML을 클라이언트에 보내 렌더링.</p>


<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://jsh99.github.io/Web/CSR_and_SSR/">https://jsh99.github.io/Web/CSR_and_SSR/</a></li>
</ul>
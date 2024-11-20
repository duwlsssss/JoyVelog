<p>김민태 강사님의 실시간 강의를 수강하며 배운 점을 정리해보겠습니다.</p>
<h3 id="✅-react에서-css를-적용시키는-방법">✅ React에서 CSS를 적용시키는 방법</h3>
<h4 id="1-inline">1. inline</h4>
<ul>
<li>html 태그 안에 style 속성을 직접 선언하는 방식</li>
<li>해당 jsx와 같은 파일 내에, style 객체 변수 선언 후 태그 안에 변수명 집어넣기로 좀 더 간편하게 가능</li>
<li>객체의 값을 좀 더 다양하게 선언하면, 상황에 맞는 값을 유연하게 inline 형태로 집어넣을 수 있음</li>
<li>속도가 굉장히 빨라, 퍼포먼스적인 부분에서도 도움이 됨</li>
<li>의사코드(클래스)를 쓸 수 없다는 단점이 있음(ex: hover, media-query, …) ⇒ 보통 큰 프로젝트가 아닌, 작은 프로젝트에서 많이 씀</li>
</ul>
<h4 id="2-normal">2. normal</h4>
<ul>
<li>import를 사용하는 방법</li>
<li>javascript 자체에서는 import 문법을 제공하지 않음 ⇒ bundler가 해석기를 제공함<ul>
<li>bundler가 javascript에서 필요한 만큼의 css만 제공</li>
</ul>
</li>
<li>link 태그를 이용하는 방법</li>
<li><em>전역 범위*</em>로 스타일이 적용되기 때문에 <strong>네이밍 충돌</strong>이 발생할 수 있음</li>
</ul>
<h4 id="3-css-in-js">3. css-in-js</h4>
<ul>
<li>css와 component 개념을 통합하려는 시도</li>
<li>대표적으로 styled component</li>
<li>styled component 형태로 변수를 선언하며, 해당 변수를 component처럼 사용 가능</li>
<li>선언하는 그 자체로 격리가 됨</li>
<li>선언할 때, css 형태 그대로 선언 가능(로직 필요 x)</li>
<li>예상치 못한 곳에 css에 적용되는 상황이 방지 가능</li>
<li>css 확장 문법 및 props를 받도록 할 수도 있음(활용성👍)</li>
<li>실행하는 순간에 css의 적용이 확정돼(만들어져 있는 파일을 전달하지 x) 속도가 느려짐(UI가 만들어질 때마다 css를 다시 적용해야 하니까)</li>
</ul>
<h4 id="4-css-module">4. css module</h4>
<ul>
<li>bundler가 제공하는 방식</li>
<li><strong>로컬 범위의 스타일</strong>을 제공하여 클래스명 충돌을 방지
bundler가 고유한 클래스명을 생성해 전역 네임스페이스에서 충돌하지 않도록 함</li>
</ul>
<p>➕ <a href="https://tailwindcss.com/docs/installation">tailwind css(css library)</a></p>
<ul>
<li>css에 관한 모든 문법에 대한 커스텀 문법이 다 세팅되어 있고, 클래스 또한 미리 만들어져 있음</li>
<li>문법에 대해 어느 정도 적응이 되면, 스타일 지정을 매우 빠르게 할 수 있음</li>
<li>단점<ul>
<li>커스텀 문법을 배워야 함</li>
<li>해당 css를 쓰지 않는 개발자와의 소통이 힘듬(협업의 부재)</li>
<li>한번 사용하면, 다른 css로 수정이 힘듦</li>
</ul>
</li>
</ul>
<hr />
<h3 id="✅-개발-용어">✅ 개발 용어</h3>
<ul>
<li><h4 id="load">load</h4>
<p>컴퓨터의 물리적 저장소의 데이터(실행 불가능)를 메모리(메인 저장 장치)에 옮겨서 <code>실행 가능한 상태</code>로 만드는 것</p>
</li>
<li><h4 id="fetch">fetch</h4>
<ul>
<li>필요한 데이터를 <strong>특정 위치</strong>에서 가져오는 작업</li>
<li>가져온 데이터를 사용하는 주체가 용도에 맞게 적절하게 씀</li>
<li>왜 api로 데이터를 가져오는 것을 fetch라고 하는가?<ul>
<li>fetch는 load와 달리 <code>실행된다는 것을 강요하지 않음</code></li>
<li>데이터를 불러오지만, 실행성과 용도가 정의되지 않기 때문</li>
</ul>
</li>
</ul>
</li>
<li><h4 id="buffercache">buffer/cache</h4>
<ul>
<li><p>데이터를 효율적으로 관리하기 위한 메모리 저장소</p>
</li>
<li><h4 id="buffer">buffer</h4>
<ul>
<li><strong>데이터를 임시 저장</strong>하는 메모리 공간으로, 주로 <strong>데이터의 이동 속도를 맞추기 위해</strong> 사용</li>
<li>선형적인 흐름으로 데이터 전달할 때, 주로 load에서 많이 쓰임
e.g. <strong>스트리밍</strong>: 네트워크에서 데이터를 스트리밍할 때, 한꺼번에 모든 데이터를 받는 대신 버퍼를 통해 데이터를 점진적으로 로드</li>
</ul>
</li>
<li><h4 id="cache">cache</h4>
<ul>
<li>원본 데이터를 자주 호출하는 대신, 캐시에 자주 사용하는 데이터를 미리 저장해두고 캐시에서 직접 데이터를 불러와 작업 부하 경감</li>
<li>주로 fetch 과정에서 많이 쓰임</li>
</ul>
</li>
</ul>
</li>
</ul>
<hr />
<h3 id="✅-redux">✅ Redux</h3>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/4e513579-ba75-43f3-9e48-07e5688e665c/image.png" /></p>
<ul>
<li>UI에서 다뤄지는 영역<ul>
<li>Business logic</li>
<li>data handling logic</li>
<li>async</li>
<li>style</li>
</ul>
</li>
</ul>
<p>⇒ MVC(model/view/controller) 패턴으로 소프트웨어 아키텍쳐 분석</p>
<ul>
<li><strong>UI</strong> - View
사용자에게 데이터를 보여줌</li>
<li><strong>Model</strong> - business logic, data handling logic
데이터 정의, 데이터 저장/불러오기 등
서버의 데이터 구조를 model이 그대로 따르는 경우가 많음</li>
<li><strong>controller</strong> - view와 model 사이의 관계(중간 다리 역할)
사용자의 요청에 따라 데이터를 처리하여 적절한 뷰에 데이터 전달</li>
</ul>
<ul>
<li>MVVM(Model - view - view model)<ul>
<li>기존의 모델을 view에 최적화된 형태로 view model로 재구성</li>
<li>view model을 바탕으로 view 구성</li>
<li>리액트와는 관련이 없음.(리액트에서 지원하지 않음/Angular에서는 지원)</li>
<li>데이터가 바뀌면 view가 자동으로 바뀌도록 자동화시킴</li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/fc704ffd-2c95-4807-9495-74c589187f62/image.png" /></p>
<ul>
<li><p>여러 컴포넌트에 대한 재사용성을 고려해, 데이터를 관리하기 쉽도록 공통 부모를 가지는 상위 컴포넌트(context api의 경우 provider)에 데이터를 저장</p>
</li>
<li><p>여러 재사용된 컴포넌트에 전달된 값을 수정하기 위해, business logic(reducer)도 마찬가지로 공통 부모(상위 컴포넌트에 저장)_Redux에서는 reducer를 통해 상태와 비즈니스 로직을 한 곳에서 처리</p>
</li>
<li><p>값에다 객체를 담을 때, 어떤 위치에서든지 수정이 가능하면 어디서 바뀌었는지 찾기 힘들고, UI가 복잡해짐 =&gt; 객체를 직접 수정하지 않고, dispatch가 action(수정된 값을 business logic에 반영하는 행위)을 수행하며 상태를 변경하며 불변성을 유지함</p>
<ul>
<li>action 매개변수를 dispatch함수에게 전달하면, 이를 바탕으로 reducer 함수를 호출하고, reducer의 리턴값을 바탕으로 store 값 수정</li>
<li>action 인자의 조건: 객체여야 하고, type을 가져야 함</li>
<li>type 의 값은 문자열인데, 이 문자열들을 어떤 return 값을 가질지 판별해주는 역할</li>
</ul>
</li>
</ul>
<br />

<ul>
<li><p>상태 관리의 흐름</p>
<ul>
<li>React State → redux → react-redux → context api → hooks</li>
</ul>
</li>
<li><p>라이브러리의 계보</p>
<ol>
<li><p>jQuery(2000년대)</p>
<ul>
<li>현재까지도 많이 쓰기는 함</li>
<li>DOM을 간단하게 다루게 해줌</li>
</ul>
</li>
<li><p>backbonejs</p>
<ul>
<li>자체적으로 틀(frame)을 제공</li>
<li>mvc 패턴 기반</li>
</ul>
</li>
<li><p>angular.js</p>
<ul>
<li><p>MVVM 구조</p>
</li>
<li><p>2way 방식</p>
<ul>
<li>사용자에 의해 상호작용하여 바뀌는 데이터를 UI로 바로 반영해줌</li>
</ul>
</li>
<li><p>2way의 복잡한 방식을 개선하기 위해, 많은 새 버전이 나옴</p>
<p>  ex) RxJs, typescript</p>
</li>
</ul>
</li>
<li><p>React</p>
<ul>
<li>1 way 방식</li>
</ul>
</li>
</ol>
</li>
</ul>
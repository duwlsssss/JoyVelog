<ul>
<li><p>태그 가져오기</p>
<p>  <strong>document.getElementById(id)</strong> : id로 한개의 요소 선택, 해당 id는 문서에서 유일해야함</p>
<p>  <strong>document.getElementsByClassName(className)</strong> : class name으로 여러 요소 선택, 반환값은 HTMLCollection임(유사배열객체)</p>
<p>  <strong>document.getElementsByTagName(tagName)</strong> : 태그이름으로 여러 요소 선택, 반환값은 HTMLCollection</p>
<p>  <strong>document.querySelector(selector)</strong> : css 선택자(클래스, 태그, id 모두 사용 가능)로 일치하는 첫번째 요소를 반환, 없으면 null 반환</p>
<p>  <strong>document.querySelectorAll(selector)</strong> : css 선택자로 일치하는 모든 요소를 반환, 반환값은 NodeList(배열)</p>
<pre><code class="language-jsx">  &lt;div id="myElement"&gt;Hello&lt;/div&gt;
  &lt;div class="myClass"&gt;World&lt;/div&gt;
  &lt;div class="myClass"&gt;JavaScript&lt;/div&gt;
  &lt;div class="myClass"&gt;DOM&lt;/div&gt;

  const elements = document.querySelectorAll('.myClass');
  elements.forEach((el) =&gt; console.log(el.textContent));
  /* 
      "World"
      "JavaScript"
      "DOM"
  */</code></pre>
</li>
</ul>
<br />

<ul>
<li>이벤트 리스너(이벤트 핸들러)추가하기</li>
</ul>
<p>특정 요소에서 지정한 타입의 이벤트가 발생하면 웹브는 그 요소에 등록된 이벤트 리스너를 실행시킴</p>
<p>EventTarget.<strong>addEventListner</strong>(type, listner [, options or useCapture]) : </p>
<p>  listner는 지정한 타입의 이벤트 발생시 실행할 이벤트 리스너로 콜백함수 or 콜백으로 작동할 handleEvent() 메서드를 포함하는 객체임</p>
<p>  options는 이벤트 수신기의 특징을 지정하는 객체 / capture(boolean, 자손으로 이벤트가 전달되기 전 이 수신기가 먼저 발동돼야함을 나타냄, default는 false), once(boolean, 수신기가 최대 한 번만 동작해야 함을 나타냄, default는 false), passive(boolean, 이 수신기 내에서 <a href="https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault"><code>preventDefault()</code></a>를 절대 호출하지 않을 것임을 나타냄, default는 브라우저마다 다름), signal(AbortSignal로 특정 조건에서 이벤트 리스너를 취소할 수 있게 함, <code>abort()</code>가 호출되면, 이벤트 리스너는 <strong>즉시 제거됨 ,</strong>default는 이벤트 수신기가 아무 AbortSignal과 연결되지 않음)
useCapture는 이벤트 대상의 DOM 트리 하위에 위치한 자손 EventTarget으로 이벤트가 전달되기 전에, 이 수신기가 먼저 실행돼야함을 나타냄, boolean값, default는 false(버블링 방식)</p>
<p>반환값 undefinded</p>
<pre><code class="language-jsx">const button = document.getElementById('myButton');

// 콜백함수 listner 인자로 넘김 
function handleClick() {
  console.log('Button clicked!');
}

button.addEventListener('click', handleClick);



// handleEvent 메서드를 가진 객체를 listner 인자로 넘김 
const eventHandler = {
   handleEvent(event) {
    switch (event.type) {
      case 'click':
        console.log('Button was clicked');
        break;
      case 'mouseover':
        console.log('Mouse is over the button');
        break;
    }
  }
};

// 이벤트 리스너 추가
button.addEventListener('click', eventHandler, { capture: true, once: true, passive: true });
button.addEventListener('mouseover', eventHandler, { capture: true, once: true, passive: true });</code></pre>
<br />

<ul>
<li>이벤트 리스너 제거하기</li>
</ul>
<p>eventTarget.<strong>removeEventListener</strong>(type, listner [, options or useCapture]) :</p>
<p>이전에 등록했던 이벤트 리스너와 옵션을 적어야 함 </p>
<pre><code class="language-jsx">button.removeEventListner('click', handleClick);
button.addEventListener('mouseover', eventHandler, {cpature:true, once:true, passive:true});</code></pre>
<br />

<ul>
<li><p>키보드와 마우스 이벤트
사용자가 키보드, 마우스를 조작할때 발생하는 이벤트를 이벤트 리스너를 통해 감지해 원하는 동작 수행, 각 이벤트는 특정한 이벤트 객체를 생성함 </p>
<ul>
<li><strong>마우스 이벤트</strong> 종류<ul>
<li>click 왼클릭</li>
<li>dblclick</li>
<li>mousedown 마우스를 누른 상태</li>
<li>mouseup 눌렀다 뗌</li>
<li>mouseover</li>
<li>mouseout</li>
<li>mousemove 마우스가 움직일때마다</li>
<li>contextmenu 우클릭</li>
</ul>
</li>
<li>마우스 이벤트 객체의 속성<ul>
<li>button 누른 마우스 버튼 0왼,1가,2오</li>
<li>clientX, clientY 뷰포트(브라우저) 기준 마우스 포인터의 좌표</li>
<li>pageX, pageY 스크롤을 고려한 전체 페이지에서 마우스 포인터의 좌표</li>
<li>screenX, screenY 모니터 화면 전체 기준 마우스 포인터 좌표</li>
<li>altKey, ctrlKey, shiftKey : 보조키 눌렸는지 여부</li>
</ul>
</li>
<li><strong>키보드 이벤트</strong> 종류<ul>
<li>keydown</li>
<li>keyup</li>
</ul>
</li>
<li>키보드 이벤트 객체 주요 속성<ul>
<li>key 눌린 키 값 _A, Enter</li>
<li>code 눌린 키의 코드값 _KeyA, Enter</li>
<li>altKey, ctrlKey, shiftKey, metaKey: 보조키 눌렸는지 여부, meta는 command</li>
</ul>
</li>
</ul>
<pre><code class="language-jsx">const button = document.getElementById('myButton');

button.addEventListener('click', (event) =&gt; {
    console.log('Button clicked');
  }
});

document.addEventListener('keydown', (event) =&gt; {
  if (event.key === 'a' &amp;&amp; event.ctrlKey) {
    console.log('Ctrl + A pressed');
  }
});</code></pre>
</li>
</ul>
<br />

<ul>
<li>태그 속성 다루기</li>
</ul>
<p>elementTarget.<strong>hasAttribute</strong>(attribute) 속성 유무 확인</p>
<p>elementTarget.<strong>getAttribute</strong>(attribute) 속성 읽기</p>
<p>elementTarget.<strong>setAttribute</strong>(attribute, value) 속성 추가/수정</p>
<p>elementTarget.<strong>removeAttribute</strong>(attribute) 속성 제거</p>
<p>elementTarget.<strong>attributes</strong> 속성들 배열 반환</p>
<pre><code class="language-jsx">const element = document.querySelector('myImg');

const srcValue = element.getAttribute('src');  

element.setAttribute('src', 'image.jpg'); 

element.removeAttribute('alt');</code></pre>
<p>-DOM 프로퍼티로 직접 접근_표준 속성의 경우</p>
<pre><code class="language-jsx">const element = document.querySelector('myImg');

// id 속성 접근
console.log(element.id);

// id 속성 수정
element.id = 'newId';

// className 속성 수정
element.className = 'newClass';</code></pre>
<p>-classList를 이용한 클래스 조작</p>
<p>class 속성은 classList 프로퍼티로 클래스 추가, 제거, 토글 등을 할 수 있음</p>
<pre><code class="language-jsx">const element = document.querySelector('myElement');

// 클래스 추가
element.classList.add('newClass');

// 클래스 제거
element.classList.remove('oldClass');

// 클래스 토글 (있으면 제거, 없으면 추가)
element.classList.toggle('active');

// 클래스 존재 여부 확인
if (element.classList.contains('active')) {
  console.log('Element is active');
}</code></pre>
<br />

<ul>
<li>부모와 자식 태그 찾기</li>
</ul>
<p>js의 DOM 트리 탐색 메서드로 특정요소의 부모, 자식, 형제 요소를 쉽게 찾을 수 있음 </p>
<ul>
<li><p>부모 요소 찾기</p>
<p>elementTarget.<strong>parentNode</strong> : 해당 요소의 직접적인 부모요소 반환(요소 노드,문서 노드, 텍스트 노드 등 포함) / 노드의 부모 노드가 꼭 요소(Element)일 필요는 없을 때 사용</p>
<p>elementTarget.<strong>parentElement</strong> : 부모가 요소 노드일 경우만 반환, 텍스트나 다른 노드이면 null 반환</p>
</li>
</ul>
<ul>
<li><p>자식 요소 찾기</p>
<p>elementTarget.<strong>children</strong>: 해당 요소의 모든 자식 요소를 HTMLCollection으로 반환, 자식 노드 중 요소 노드만 포함됨 </p>
<p>elementTarget.<strong>firstElementChild</strong>: 첫번째 자식 요소 반환, 없으면 null </p>
<p>elementTarget.<strong>lastElementChild</strong>: 마지막 자식 요소 반환, 없으면 null </p>
<p>elementTarget.<strong>querySelector</strong>: CSS 선택자를 사용해 첫 번째로 일치하는 <strong>자식 요소</strong>를 반환</p>
<p>elementTarget.<strong>querySelectorAll</strong>: CSS 선택자를 사용해 일치하는 모든 <strong>자식 요소</strong>를 NodeList로 반환</p>
<p>elementTarget.<strong>childNodes</strong>: 모든 자식 노드를 NodeList로 반환함, 텍스트 노드나 다른 노드도 포함됨 </p>
<pre><code class="language-jsx">const parent = document.getElementById('myParentElement');

const children = parent.children;

const firstChild = parent.firstElementChild;

const lastChild = parent.lastElementChild;

const specificChild = parent.querySelector('.childClass');

const specificChildren = parent.querySelectorAll('.childClass');

const allChildren = parent.childNodes;</code></pre>
</li>
<li><p>형제 요소 찾기</p>
<p>elementTarget.<strong>nextElementSibling</strong>: 해당 요소의 다음 형제 요소 반환</p>
<p>elementTarget.<strong>previousElementSibling</strong>: 해당 요소의 이전 형제 요소 반환 </p>
</li>
</ul>
<br />

<ul>
<li>새로운 태그 만들기</li>
</ul>
<p>document.<strong>createElement</strong>(tagName) 주어진 태그이름으로 새로운 요소를 생성하여 반환</p>
<p>elementTarget.<strong>appendChild</strong>(Node) 인자로 전달한 노드를 메소드를 호출한 요소의 마지막 자식으로 추가 → 메소드를 호출한 요소 아래로 형제 관계 형성, 추가한 노드 반환</p>
<p>elementTarget.<strong>insertBefore</strong>(newNode, childNode) 첫번째 인자 요소를 두번째 인자 요소 바로 앞에 삽입함 / 두번째 인자는 이 메서드를 호출한 요소의 자식이어야 함</p>
<p>elementTarget.<strong>append</strong>(newNode) 메소드 호출한 요소의 마지막 자식요소 뒤에 추가, 반환값 undefined</p>
<p>elementTarget.<strong>prepend</strong>(newNode) 메소드 호출한 요소의 첫번째 자식요소 앞에 추가 → 메소드를 호출한 요소 아래로 형제 관계 형성</p>
<p>elementTarget.<strong>before</strong>(newNode) 선택한 요소의 앞에 추가 → 메소드를 호출한 요소의 형제 노드로 추가됨, 반환값 undefined</p>
<p>elementTarget.<strong>after</strong>(newNode) 선택한 요소의 뒤에 추가</p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/816d690c-a55f-4bd0-a1b8-dbd354d70576/image.png" /></p>
<pre><code class="language-jsx">const newButton = document.createElement('button');

newButton.textContent = 'Click Me';

newButton.className = 'btn btn-primary';
newButton.setAttribute('type', 'button');

newButton.addEventListener('click', () =&gt; {
  alert('Button clicked!');
});

const parentDiv = document.getElementById('parentDiv');

parentDiv.append(newButton);</code></pre>
<br />

<ul>
<li>태그 복제하기</li>
</ul>
<p>elementTarget.<strong>cloneNode</strong>(boolean): 현재 노드를 복사해 반환, 이벤트 리스너는 복사되지 않음 → 복제된 요소에 이벤트 리스너 필요하면 다시 추가해줘야함, 복제된 노드는 메모리에만 존재해 DOM에 이를 추가해줘야 함</p>
<p>인자가 true 면 노드와 그 안의 모든 자식 노드가 복사됨 
false(default)면 노드 자체만 복사되고 자식 노드는 복사되지 않음</p>
<pre><code class="language-jsx">const originalButton = document.getElementById('myButton');

// 깊은 복사로 요소 복제
const clonedButton = originalElement.cloneNode(true);

// 복제된 버튼에 이벤트 리스너 추가
clonedButton.addEventListener('click', () =&gt; {
  alert('Cloned button clicked!');
});

// 복제된 요소를 DOM에 추가
document.body.append(clonedElement);</code></pre>
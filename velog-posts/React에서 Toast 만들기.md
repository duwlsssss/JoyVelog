<h3 id="🎨-배경">🎨 배경</h3>
<p>백그라운드 리패치 오류나 네트워크 연결에 여러 번 실패할시 토스트로 피드백을 주려고 했다.</p>
<p>컴포넌트를 import해 사용하는 것보다 util함수로 만들어 window.alert처럼 메시지만 넣어 간단히 호출하게 하면 팀원들이 사용하기 편할 것 같았다.</p>
<p>전역 상태 관리 라이브러리로 <code>zustand</code>를 사용하고 있어 이를 활용해 토스트 오픈, 애니메이션 상태를 관리하고자 했다.</p>
<h3 id="구현-과정">구현 과정</h3>
<p>1️⃣ <strong>전역 상태 설정</strong></p>
<pre><code class="language-jsx">import { create } from 'zustand';

const useToastStore = create((set) =&gt; ({
  isOpen: false,
  message: '',
  isFadingOut: false, // 애니메이션 상태
  show: (message: string) =&gt; {
    set({ isOpen: true, message, isFadingOut: false });
    setTimeout(() =&gt; set({ isFadingOut: true }), 2500); // 2.5초 후 fadeout 시작
    setTimeout(() =&gt; set({ isOpen: false, isFadingOut: false }), 3000); // 3초 후 toast 숨김
  },
  close: () =&gt; set({ isOpen: false, message: '', isFadingOut: false }),
}));

export default useToastStore;</code></pre>
<p><code>setTimeout</code>으로 특정 시간이 지나면 페이드 아웃하고, 더 지나면 사라지게 할 것이다.</p>
<p>2️⃣ <strong>util 함수</strong></p>
<pre><code class="language-js">import useToastStore from '../stores/ui/useToastStore';

const toast = (message) =&gt; {
  const store = useToastStore.getState();
  // 이미 토스트가 표시 중이라면 새로운 요청 무시
  if (store.isOpen) {
    return;
  }
  store.show(message);
};

// 토스트를 강제 종료하는 함수
export const closeToast = () =&gt; {
  const store = useToastStore.getState();
  store.close();
};

export default toast;</code></pre>
<p>토스트가 중복 생성되지 않도록 isOpen 상태를 확인해 show를 호출한다.</p>
<p>3️⃣ *<em>Toast 렌더링용 DOM 노드 생성 *</em></p>
<pre><code>&lt;body&gt;
    &lt;div id=&quot;root&quot;&gt;&lt;/div&gt;
    &lt;div id=&quot;modal-portal&quot;&gt;&lt;/div&gt;
    &lt;div id=&quot;toast-portal&quot;&gt;&lt;/div&gt;
    &lt;script type=&quot;module&quot; src=&quot;/src/main.jsx&quot;&gt;&lt;/script&gt;
  &lt;/body&gt;</code></pre><p>레이어를 분리하기 위해 <code>createPortal</code>을 사용할 것이다.</p>
<p><code>createPortal(children, domNode, key?)</code>
일부 JSX와 렌더링할 DOM 노드를 전달해 사용한다.</p>
<p>그럼 React는 사용자가 전달한 JSX에 대한 DOM 노드를 사용자가 제공한 DOM 노드 안에 넣는다.
이런식으로 JSX를 순간 이동시킬 수 있다.</p>
<p>이를 위해 id가 toast-portal인 노드를 추가했다.</p>
<p>4️⃣ <strong>Toast component</strong></p>
<pre><code class="language-jsx">import * as S from './style';
import { createPortal } from 'react-dom';
import { AnimatePresence } from 'framer-motion';
import useToastStore from '@/stores/ui/useToastStore';

const Toast = () =&gt; {
  const { isOpen, message, isFadingOut } = useToastStore();

  if (!isOpen) return null;

  const toastRoot = document.getElementById('toast-portal');

  return createPortal(
    &lt;AnimatePresence&gt;
      {isOpen &amp;&amp; (
        &lt;S.Toast
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: isFadingOut ? 0 : 1, y: isFadingOut ? 50 : 0 }}
          exit={{ opacity: 0, y: 50 }}
          transition={{ duration: 0.5 }}
          role=&quot;alert&quot;
          aria-live=&quot;assertive&quot;
        &gt;
          {message}
        &lt;/S.Toast&gt;
      )}
    &lt;/AnimatePresence&gt;,
    toastRoot,
  );
};

export default Toast;</code></pre>
<p><a href="https://www.npmjs.com/package/framer-motion">framer-motion</a>을 사용해 애니메이션을 설정했다.
렌더링되면 y 값이 50에서 0으로 바뀌게 해 밑에서 올라오는 효과를 줬다.
<code>isFadingOut</code>이 true가 되면 다시 50으로 이동한다.</p>
<p>스타일은 따로 분리해놨다.</p>
<pre><code class="language-js">import styled from 'styled-components';
import { motion } from 'framer-motion';

export const Toast = styled(motion.div)`
  position: fixed;
  z-index: 2;
  right: 16px;
  bottom: 16px;

  display: flex;
  align-items: center;
  justify-content: center;

  padding: 16px 24px;
  border-radius: 5px;

  color: #555555;
  text-align: center;
  white-space: nowrap;

  box-shadow: 0 2px 5px rgb(0, 0, 0, 20%);
`;</code></pre>
<p>그리고 App.jsx에 Toast 컴포넌트를 추가해놨다</p>
<pre><code class="language-jsx">function App() {
  // ...
  &lt;Suspense fallback={&lt;DeferredLoader /&gt;}&gt;
    &lt;Toast /&gt;
    &lt;Router /&gt;
  &lt;/Suspense&gt;
  // ...
}</code></pre>
<h3 id="🚀-결과">🚀 결과</h3>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/e40c0d6e-96cc-44bf-bcb9-971a883be36b/image.gif" /></p>
<p>모달을 단순히 컴포넌트로 사용해왔는데 이런 방식으로 구현하니 편하게 사용할 수 있었다.
alert와 confirm도 직접 만들어 사용할 계획이다.</p>
<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://ko.react.dev/reference/react-dom/createPortal">https://ko.react.dev/reference/react-dom/createPortal</a></li>
</ul>
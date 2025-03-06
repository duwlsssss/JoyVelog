<p>피클 프로젝트에서 원하는 이벤트를 검색한 뒤 목록을 볼 수 있어야 했습니다. 디자이너분이 네이버 지도를 참고해 bottom sheet 형태로 목록을 볼 수 있게 디자인을 수정해줬습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/748cd550-7a49-4926-ba93-edc1760b41d7/image.gif" /></p>
<p>react에서 bottom sheet를 어떻게 구현했는지 적어보겠습니다.</p>
<p><code>framer-motion</code>을 사용해 헤더 영역을 설정하고 그 부분의 y축 수직 드래그를 반영해 바텀 시트를 열고 닫도록 구현했습니다.</p>
<p>열린 상태이면 top을 검색 헤더 높이로 설정해 이 부분을 제외한 부분을 채우게 했고
닫힌 상태이면 바텀시트의 헤더 부분만 표시되게 했습니다.</p>
<pre><code class="language-js">const bottomSheetVariants = {
  opened: { top: 'var(--search-header-height)' },
  closed: { top: `calc(100dvh - var(--search-bottom-sheet-height))` },
};</code></pre>
<p>먼저 드래그를 반영하기 위한 임계값을 설정했습니다.</p>
<pre><code class="language-js">const offsetThreshold = 100;
const deltaThreshold = 5;</code></pre>
<p><code>offsetThreshold</code>는 드래그가 시작된 후, 드래그된 거리의 절대값이 이 값을 초과해야 드래그 상태를 변경합니다. 즉, 사용자가 화면을 얼마 이상 드래그하면 바텀 시트가 열리거나 닫힙니다.
<code>deltaThreshold</code>는 드래그 중에 발생하는 변화량의 절대값이 이 값을 초과해야 드래그 상태를 변경합니다. 이는 드래그가 너무 미세한 움직임일 경우 상태 변경을 방지하기 위한 것입니다.</p>
<p>처음엔 <code>motion.div</code> 태그에 <code>dragConstraints</code>를 top: 0, bottom: 0 으로 설정해 바텀 시트가 원하는 위치로 이동해 고정되게 하고
<code>dragElastic</code>으로 위아래로 드래그 했을때 탄력성을 줬습니다.</p>
<pre><code class="language-js">dragConstraints={{
  top: 0,
  bottom: 0,
}}
dragElastic={ 0.5 }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/04ef75d7-ceea-4372-b7cf-92e8d84e442a/image.gif" /></p>
<p>그러나 검색 헤더 위로 드래그 되는 문제가 있었고
제 경우엔 바텀시트를 올릴땐 검색 헤더를 제외한 부분을 채워야 하므로 <code>dragElastic</code>에 top: 0 을 설정해 위로 드래그되는 경우는 탄력성을 주지 않아 검색 헤더 위로 드래그 되지 않게 했습니다.</p>
<pre><code class="language-js">dragConstraints={{
  top: 0,
  bottom: 0,
}}
dragElastic={{ top: 0, bottom: 0.5 }}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/42df0b04-3e4e-4338-b0fa-dfca1b345f4a/image.gif" /></p>
<p>아래는 전체 코드입니다.</p>
<pre><code class="language-js">import {
  AnimatePresence,
  useDragControls,
  useMotionValue,
  PanInfo,
} from 'framer-motion';

const overlayVariants = {
  opened: { opacity: 1, transition: { duration: 0.2 } },
  closed: { opacity: 0, transition: { duration: 0.2 } },
};

const bottomSheetVariants = {
  opened: { top: 'var(--search-header-height)' },
  closed: { top: `calc(100dvh - var(--search-bottom-sheet-height))` },
};

const SearchBottomSheet = () =&gt; {
  const { events } = useEventsStore();
  const { isSearchBSOpen, setIsSearchBSOpen } = useSearchBottomSheetStore();

  // 드래그 상태 관리
  const dragY = useMotionValue(0);
  const animateState = isSearchBSOpen ? 'opened' : 'closed';

  const dragControls = useDragControls();

  const handleDragEnd = (_: PointerEvent, info: PanInfo) =&gt; {
    const isOverOffsetThreshold = Math.abs(info.offset.y) &gt; offsetThreshold;
    const isOverDeltaThreshold = Math.abs(info.delta.y) &gt; deltaThreshold;

    const isOverThreshold = isOverOffsetThreshold || isOverDeltaThreshold;
    if (!isOverThreshold) return;

    const newIsOpened = info.offset.y &lt; 0;

    setIsSearchBSOpen(newIsOpened);
  };

  const handleGotoMapBtnClick = () =&gt; {
    setIsSearchBSOpen(false);
  };

  return (
    &lt;&gt;
      &lt;AnimatePresence initial={false}&gt;
        &lt;S.Overlay 
          animate={animateState}
          variants={overlayVariants} 
        /&gt;
        &lt;S.BottomSheetContainer
          drag=&quot;y&quot;
          dragConstraints={{
            top: 0,
            bottom: 0,
          }}
          dragElastic={{ top: 0, bottom: 0.5 }}
          style={{y: dragY}}
          animate={animateState}
          variants={bottomSheetVariants}
          onDragEnd={handleDragEnd}
          dragControls={dragControls}
          dragListener={false}
          &gt;
          {events.length &gt; 0 &amp;&amp; (
            &lt;S.BottomSheetHeader onPointerDown={(e) =&gt; dragControls.start(e)}&gt;
              &lt;Filter isSearchPage={true} /&gt;
            &lt;/S.BottomSheetHeader&gt;
          )}
          &lt;S.BottomSheetContent&gt;
            &lt;Suspense fallback={&lt;EventListSkeleton /&gt;}&gt;
              &lt;EventList page={'search'} /&gt;
            &lt;/Suspense&gt;
            )}
          &lt;/S.BottomSheetContent&gt;
        &lt;/S.BottomSheetContainer&gt;
      &lt;/AnimatePresence&gt;
    &lt;/&gt;
  );
};

export default SearchBottomSheet;</code></pre>
<p>바텀 시트 구현이 처음이라 막막했는데 다른 분께 제 글이 도움이 됐으면 좋겠습니다 ☺️ </p>
<blockquote>
<p>참고 자료</p>
</blockquote>
<ul>
<li><a href="https://velog.io/@sangpok/React-Bottom-Sheet">https://velog.io/@sangpok/React-Bottom-Sheet</a></li>
<li><a href="https://velog.io/@nuo/React-Framer-Motion">https://velog.io/@nuo/React-Framer-Motion</a></li>
</ul>
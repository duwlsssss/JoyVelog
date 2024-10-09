<h3 id="css">CSS</h3>
<ul>
<li><p>선택자 우선순위(점수 높은 거 적용됨)</p>
<p>  <code>!important</code>_속성 값 옆에 씀 <strong>infinite</strong> / <code>인라인</code>_태그의 style 속성으로 <strong>1000</strong> / <code>#</code> 100 <strong>*<em>/ <code>.</code> , <code>:hover</code>(가상선택자), <code>::before</code>(가상 요소) *</em>10</strong> / <code>태그</code> <strong>1</strong>  / <code>*</code> <strong>0</strong> / <code>:not()</code> <strong>0 - 안에 뭘 쓰냐에 따라 달라짐</strong></p>
</li>
<li><p>width, height (defalut: auto_브라우저가 정함)</p>
<ul>
<li>max-width, max-height (default: none)</li>
<li>min-width, min-height (default: 0)</li>
</ul>
</li>
<li><p>여백 (margin, padding)</p>
<ul>
<li>margin (defalut: 0) 상하좌우 or 상하|좌우 or 상|중|하 or 상|우|하|좌</li>
<li>padding (default: 0)</li>
</ul>
</li>
<li><p>border: border-width    border-style    border-color</p>
<ul>
<li>border-radius (default:0) : r_r을 가진 원이 꼭짓점에 생김 </li>
</ul>
</li>
<li><p>box-sizing :</p>
<ul>
<li>content-box : 실제 요소의 크기는 설정한 width, height에 padding, border가 더해져 의도와 다른 크기가 될 수 있음</li>
<li>border-box: 요소의 내용+padding+border로 요소의 최종 크기 계산</li>
</ul>
</li>
<li><p>overflow (default:visible) :</p>
<ul>
<li>hidden</li>
<li>scroll_스크롤바로 가려진 내용 볼 수 있음(가로,세로)</li>
<li>auto_넘치는 부분만 스크롤바 생성</li>
</ul>
</li>
<li><p>display : (default: block or inline or inline-block_width, height 지정할 수 있는 Inline 요소)</p>
<ul>
<li>none_화면에서 사라짐</li>
<li>1차원 레이아웃<strong>flex</strong> or inline-flex //블록이나 인라인 flex container 생성</li>
<li>2차원 레이아웃<strong>grid</strong> or inline-grid //블록이나 인라인 grid container 생성</li>
</ul>
</li>
<li><p>opacity (default:0) : 0~1</p>
</li>
<li><p>font</p>
<ul>
<li>font-style (default:normal) : italic</li>
<li>font-weight defalut(normal_400) : bold_700</li>
<li>font-size (default:16px)</li>
<li>line-height: 숫자_글꼴의 숫자배</li>
<li>font-family: “글꼴-1”, …글꼴계열 (sans-serif_고딕체 계열)</li>
</ul>
</li>
<li><p>text</p>
<ul>
<li>text-align: (default:left)  //문자의 정렬 방식</li>
<li>text-decoration: (default:none) : underline or line-through  //장식 선</li>
<li>text-indent: (default:0) rem //들여쓰기</li>
<li>color (default:rgb(0,0,0))</li>
</ul>
</li>
<li><p>background</p>
<ul>
<li>background-color: (default: transparent) 색상 //배경색상은 배경이미지 뒤에 나옴</li>
<li>background-image: (default: none) url(”경로”)</li>
<li>background-size: (default: auto_실제크기) 단위(px,em,rem) or cover(비율유지, 이미지 가로세로 중 더 짧은 너비에 맞춤, 이미지의 일부가 잘릴 수 있음) or contain(비율유지, 더 넓은 너비에 맞춤, 요소 내에 빈 공간이 생길 수 있음)</li>
<li>background-repeat: (default: repeat)  no-repeat or repeat-x or repeat-y //이미지를 수직이나 수평으로 반복</li>
<li>background-position: 방향(top,bottom,left,right_여러개 사용 가능, <strong>center</strong>) or 단위 //배경 이미지의 위치</li>
<li>backgroud-attachment: (defalut:scroll) fixed(화면스크롤되도 배경 이미지는 고정됨) //화면 스크롤시 요소가 올라가거나 내려갈때 배경 이미지도 같이 움직일지 여부</li>
</ul>
</li>
<li><p>배치 (position, z-index)</p>
<ul>
<li><p>position: (default: static_기준 없음) relative(요소 자신) absolute(위치상 부모요소) fixed(뷰포트) //요소의 위치 지정 기준</p>
<ul>
<li><p>relative를 설정 후 이동하는 건 사용하지 않음-요소가 document flow(페이지 레이아웃)에서 차지하는 공간은 그대로 남겨두고, 시각적으로만 이동해 다른 요소들과의 레이아웃 충돌을 일으킬 수 있음</p>
</li>
<li><p>absolute는 Document Flow에 제외 되므로 위치상 부모요소를 기준으로 원하는 위치로 요소를 자유롭게 이동 가능 / 부모요소는 static이 아닌 <strong>relative</strong>, fixed, absolute 중 하나여야 함</p>
</li>
<li><p>fixed는 뷰포트를 기준으로 위치를 지정해 스크롤을 해도 고정된 위치에 표시됨</p>
</li>
<li><p>position에 absolute, fixed를 주면 display block이 됨 </p>
</li>
</ul>
</li>
<li><p>z-index: (default: auto_부모요소와 동일함 쌓임 정도) 숫자</p>
<ul>
<li>요소 쌓임 순서: position이 있는거(static제외) -&gt; z-index가 높은거 -&gt; html 구조에서 나중에 작성된거</li>
</ul>
</li>
</ul>
</li>
<li><p>전환 (transition)</p>
<p>  transition: transition-property  <strong>transition-duration</strong>  transition-timing-function  transition-delay // 요소의 전환(시작, 끝) 효과를 지정하는 단축 속성</p>
<ul>
<li>transition-property: (default: all_모든 속성에 적용) 속성 이름 //전환 효과를 사용할 속성의 이름을 지정</li>
<li>transition-duration: (default: 0s_전환 효과 없음) 전환효과 지속시간(s)</li>
<li>transition-timing-function: (default: ease_느리게-빠르게-느리게) liner(일정하게) or ease-in(느리게-빠르게) or ease-out(빠르게-느리게) or ease-in-out(느리게-빠르게-느리게)</li>
<li>transition-delay: (default: 0s_대기 시간 없음) 대기시간(s) //전환 효과가 몇 초 뒤에 시작할지 대기시간 지정</li>
</ul>
</li>
<li><p>변환 (transform)
  transform: 변환함수1, 변환함수 2,...</p>
<ul>
<li>2D 변환함수: translate(x,y), translateX(x) ,translateY(y)_이동 x값이 양수면 오른쪽, 음수면 왼쪽으로 이동함, y값이 양수면 아래로, 음수면 위로 이동함_웹페이지에서 y는 위에서 아래로 증가, scale(요소 x,y값 조절할 값)_크기조절, rotate(deg)_회전, skew(x축 deg,y축 deg), skewX(x축 deg), skewY(y축 deg)_기울임<img src="https://velog.velcdn.com/images/kimlj0814/post/17ce3933-55f4-4b61-bddf-6df0a07e96e5/image.png" width="70%" />

</li>
</ul>
<img src="https://velog.velcdn.com/images/kimlj0814/post/87db9f46-f2ec-4294-8e1c-8e02b7e14eca/image.jpeg" width="40%" />
ㄴ요소의 안쪽에서 degree만큼 민다고 생각하자

<br />
  - 3D 변환함수: rotateX(x축 기준 회전 deg), rotateY(y축 기준 회전deg), prespective(요소를 관찰하는 원근 거리)_적용할거면 제일 먼저 적기!, 기준점은 transform-origin
  <img src="https://velog.velcdn.com/images/kimlj0814/post/7d09a83c-5400-46ce-9ce9-626deff9b194/image.png" width="80%" />
  - perspective: 단위 //하위 요소를 관찰하는 원근 거리 속성_perspective 함수와 달리 관찰 대상이 아닌 관찰 대상의 부모에 설정함, 기준점은 perspective-origin
<img src="https://velog.velcdn.com/images/kimlj0814/post/fa594689-9cde-4114-b3a9-eb4eb148d0c5/image.png" />
  - backface-visibility: (default: visible) hidden //3D 변환으로 회전된 요소의 뒷면 보임 여부
<br /><br /></li>
</ul>
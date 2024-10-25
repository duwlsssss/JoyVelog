<p>부트캠프 첫번째 프로젝트 후기를 남겨보려고 합니다.</p>
<p>팀이 랜덤으로 구성돼서 피어세션을 하며 아이스브레이킹을 했습니다. 저희 팀은 대부분이 전공자로 구성돼있고 현업을 경험하셨던 분도 있어서 굉장히 든든했습니다. 더 열심히 도움이 되고 싶은 마음이 생겼어요ㅎㅎ
다들 열정있고 경험도 조금 있어서 기획도 빠른 속도로 진행됐습니다 👍👍</p>
<p>이번 토이 프로젝트는 <code>인트라넷 서비스</code>를 개발하는 것입니다.
관리자용 페이지, 사용자용 페이지를 나눠 필요한 기능을 구현해야했습니다.</p>
<p>꼭 들어가야 하는 기능은 제공됐습니다. 
그래서 저희는 첫날엔 이슈 템플릿, pr 템플릿, project같은 깃허브 설정을 해두고 코드 컨벤션을 정했습니다. </p>
<p>코드 컨벤션을 열심히 정해 두고 그냥 안 지키면 의미가 없기 때문에 설정한 정책을 강제적으로 지키도록 하고 그렇지 않을 경우 merge, commit과 같은 과정에서 작업을 차단하는 git hook을 사용하기로 했습니다.</p>
<p>git hook은 설정이 복잡해 이걸 쉽게 도와주는 npm 모듈인 husky가 있는데요. 팀원 2명이 써봤다 했는데 다같이 배우는 입장이니 안 써 본 사람이 적용해보자는 말이 나왔어요. 저는 Github를 무서워해서ㅋ(<del>최근에 코드 날아간 경험 있음</del>) 눈치 보다가 뭐든 배우고 싶어 제가 해보겠다고 자원했습니다!!</p>
<p>처음엔 commit, push를 시도할 때 각각 prettier, ESLint 규칙을 체크하려했는데 
lint 규칙에 위반되는 걸 고치고 체크하려면 계속 커밋해야해서 불필요한 커밋이 쌓였습니다.
그래서 커밋 전에 Prettier와 ESLint 규칙을 모두 체크하고,
커밋 후에는 푸시할 때 추가적인 규칙 체크 없이 푸시가 되도록 했습니다.</p>
<ol>
<li>ESLint, prettier 설정</li>
<li>husky 설치: <code>npm i husky --save-dev</code></li>
<li>lint-staged 설치: <code>npm install lint-staged --save-dev</code></li>
<li>Git hooks와 Husky를 연결: <code>npx husky install</code></li>
<li>package.json의 sripts에<pre><code class="language-jsx">"scripts": {
"prepare": "husky install",
...
}</code></pre>
이렇게 써서 다른 팀원이 <code>npm install</code>을 하면 husky가 설치되게 합니다.</li>
<li>package.json에 아래 내용을 추가합니다.<pre><code class="language-jsx">"lint-staged": {
"*.{js,jsx}": "eslint --fix",
"*.{js,css,md}": "prettier --write"
}</code></pre>
</li>
<li>생성된 <code>.husky</code> 디렉토리 안에 <code>pre-commit</code> 파일을 생성하고<pre><code class="language-jsx">echo "🔍 commit 이전에 lint 규칙을 적용합니다..."
if npx lint-staged; then
echo "✅ 모든 lint 규칙이 성공적으로 적용되었습니다."
exit 0
else
echo "❌ lint 규칙 검사에서 오류가 발생했습니다."
exit 1
fi</code></pre>
이렇게 넣어서 commit을 시도하면 prettier과 lint 규칙을 체크하고 위반되면 commit이 중단되게 했습니다.</li>
</ol>
<p>일부로 설정한 lint 규칙에 위배되는 코드를 짰을 때
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/97763955-16b4-4957-acd6-05e7153d369c/image.png" /></p>
<p>수정하고 다시 커밋하면
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/9c2ce781-0043-40bc-aa0a-b169eb3d754e/image.png" /></p>
<p>lint 규칙에 위반되는 코드가 많아서 다 수정하는 데 조금 힘들었습니다ㅜㅠ</p>
<p>일단 Lint 오류 해결하고 라벨 설정을 하려했는데 </p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/826e1211-3802-405b-b7bb-f07c05d459a4/image.png" /></p>
<p>언제 바꾼 거지...?? 진짜 알잘딱깔센 우리 팀 너무 짱입니다.</p>
<p>회의록도 이따가 정리해야지... 했는데
잠시 나갔다 들어가면 다 정리돼있음 ㄷㄷ</p>
<img src="https://velog.velcdn.com/images/kimlj0814/post/7eb4f80b-dada-4983-a397-41bcfe3d1228/image.png" width="80%/" />

<p>열정은 다들 만땅이지만 프론트엔드 여러명이서 하는 프로젝트는 처음이라 어떤 순서로 진행해야 하는지 혼란이 많았어요...</p>
<p>일단 개발 기간이 짧아 빠르게 피그마로 필요한 페이지, 디자인을 작성하고 css 스타일 가이드를 만들어 통일성을 주며 개발 중입니다.</p>
<p>다른 분 코드를 봤을 때 리팩토링 하고 싶다는 욕심이 생겨서 맘에 안 드는 부분 리팩토링하고, 제 몫 하고... 이렇게 반복했더니 너무 많은 시간이 걸리더라고요ㅜㅠㅜㅠ 비슷한 이상한 코드가 반복되는 걸 보니 소통의 문제도 있는 것 같고... 지금은 다들 마음이 급해서 그런 것 같은데 빨리 필수 구현 기능을 개발하고 정리할 필요가 있겠습니다.</p>
<p>서버는 다음주에 개발해 연결하기로 했는데 더욱 동적인 데이터 조작이 가능해질 것 같아 기대됩니다!!</p>
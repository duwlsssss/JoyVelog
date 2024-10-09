<h2 id="git--github">Git / Github</h2>
<p>-Git은 소프트웨어 개발 과정에서 생기는 변경사항을 시간에 따라 기록, 이전 버전과 비교하며 수정사항을 관리하는 분산 버전 관리 시스템</p>
<p>-Github는 Git의 히스토리(기록)을 온라인에 저장해주는 웹호스팅 서비스 시스템(클라우드)</p>
<p>-Git은 기본적으로 <strong>로컬 저장소</strong>를 지원하고, 협업에 필요한 <strong>원격 저장소</strong>는 Github, Gitlab 같은 서비스를 이용, 로컬 저장소가 있어 속도가 빠르고 자신의 로컬에 부담 없이 코드를 기록할 수 있으며, 원격 저장소에 문제가 생기면 로컬 저장소를 통해 복구도 가능</p>
<p>-프로젝트의 <strong>버전을 브랜치로 구분</strong>하고 자유롭게 움직일 수 있음  </p>
<p>⇒ 소스 코드 관리 도구 중 Git을 사용하는 이유는 로컬 저장소를 이용한 빠른 퍼포먼스와 브랜치를 통한 효율적인 협업임</p>
<br />

<ul>
<li><p>Git의 3가지의 컴포넌트</p>
<ul>
<li><p>Work-tree (Working Directory)</p>
</li>
<li><p><em>현재 작업중인 디렉토리, 수정가능한 모든 파일이 포함된 영역*</em></p>
</li>
<li><p>Index (Staging Area)</p>
<p>work-tree에 있는 수정된 파일 중 <strong>commit할 파일들을 임시로 저장하는 공간</strong></p>
</li>
<li><p>Repository (컨테이너)</p>
</li>
<li><p><em>모든 커밋의 history를 snapshot 형태로 저장하는 공간*</em> </p>
<ul>
<li>Local Repository: 작업 중인 컴퓨터에 저장되는 <strong>변경 내역 저장 공간</strong>_Git이 설치된 컴퓨터 저장 공간</li>
<li>Remote Repository:  <strong>변경 내역이</strong> 외부 서버에 저장되는 중앙 저장소_ Githib이 이에 해당함
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/2980f906-7141-4d2c-8c77-bad7459850e1/image.png" />
ㄴ Git의  컴포넌트들과 컴포넌트간 이동하는 명령어</li>
</ul>
</li>
</ul>
</li>
</ul>
<h2 id="basic-skills">Basic Skills</h2>
<h3 id="add">Add</h3>
<pre><code class="language-bash">git add {file name}</code></pre>
<ul>
<li>&quot;file name&quot;의 파일을 추적 및 Staged로 전환하는 명령어</li>
</ul>
<pre><code class="language-bash">git add .</code></pre>
<ul>
<li>작업 중인 모든 파일을 추가 및 Staged로 전환하는 명령어</li>
</ul>
<h3 id="commit">Commit</h3>
<pre><code class="language-bash">git commit -m &quot;Commit Message&quot;

git commit</code></pre>
<p><code>git commit</code>을 CLI에 입력면, vi 편집기를 사용한 편집모드에 들어가게 된다.</p>
<h3 id="log">Log</h3>
<pre><code class="language-bash">git log</code></pre>
<p>과거 버전을 확인할 수 있다.</p>
<h3 id="branch">Branch</h3>
<pre><code class="language-bash">git branch</code></pre>
<p>로컬에서 사용 중인 브랜치를 확인하는 명령어</p>
<pre><code class="language-bash">git branch --remote</code></pre>
<p>원격 레포지토리에서 사용 중인 브랜치를 확인하는 명령어</p>
<pre><code class="language-bash">git branch {New Branch Name}</code></pre>
<p>&quot;New Branch Name&quot;이라는 이름의 브랜치를 생성하는 명령어</p>
<h3 id="checkout">Checkout</h3>
<pre><code class="language-bash">git checkout {Branch Name}</code></pre>
<ul>
<li>&quot;Branch Name&quot;의 브랜치로 이동하는 명령어</li>
</ul>
<pre><code class="language-bash">git checkout -b {New Branch Name}</code></pre>
<ul>
<li>새로운 브랜치 &quot;New Branch Name&quot;으로 개설하면서 이동하는 명령어</li>
</ul>
<h3 id="remote">Remote</h3>
<pre><code class="language-bash">git remote --v</code></pre>
<p><code>.git</code> 파일이 있는 디렉터리가 어떤 원격 레포지토리에 연결되어있는지 확인하는 명령어</p>
<h3 id="push">Push</h3>
<pre><code class="language-bash">git push origin</code></pre>
<p><code>origin</code> 원격 레포지토리로 업로드</p>
<h3 id="pull">Pull</h3>
<pre><code class="language-bash">git pull</code></pre>
<p>현재 작업 중인 브랜치에서 원격 브랜치의 버전이 더 높을 때, 변경사항을 원격에서 로컬로 가져오는 명령어</p>
<h3 id="merge">Merge</h3>
<blockquote>
<p>개발자가 &quot;Branch A&quot;에서 작업 중이고, &quot;Branch B&quot;의 변경사항을 병합한다고 가정</p>
</blockquote>
<pre><code class="language-bash">git merge {Branch B}</code></pre>
<p>&quot;Branch B&quot;의 변경사항을 병합하는 명령어</p>
<p>merge 충돌 나면
충돌 부분 해결 후 수동으로 merge 커밋 만들어 푸시</p>
<pre><code class="language-bash">git add .
git commit -m &quot;resolve merge conflict&quot;
git push origin {원격브랜치}</code></pre>
<h3 id="pull-request">Pull Request</h3>
<blockquote>
<p>Pull Request는 git의 기능이 아니고, GitHub의 기능입니다.
내 브랜치에서 작업 마치고, 다른 브랜치에 merge하기 전 팀원에게 코드 리뷰 요청, 적절하면 병합</p>
</blockquote>
<ol>
<li><strong>이슈에서 브랜치 따기</strong> <ul>
<li>깃허브의 이슈에서 Development ⚙️ → Create a branch (<strong>Branch name</strong> 수정)</li>
</ul>
</li>
</ol>
<pre><code class="language-bash">git fetch origin
git checkout joy/#{주차}</code></pre>
<ol start="2">
<li>(브랜치 생성 후 작업 중에 원격에 다른 커밋이 생기면) 그 최신상태를 반영해야 한다. 
push 전에도 하면 충돌 미리 예방 가능</li>
</ol>
<pre><code class="language-bash">git pull origin joy/main //원격 joy/main에서 최신 상태를 가져와 로컬에 반영
git push origin joy/#{주차} //병합 후 작업 브랜치를 원격에 푸시</code></pre>
<ol start="3">
<li><strong>작업 후 <code>add</code> - <code>commit</code> - <code>push</code></strong></li>
</ol>
<pre><code class="language-bash">git add .
git commit -m &quot;[week1/mission] TodoList (HTML, CSS, JS) 구현”
git push origin joy/#{주차}</code></pre>
<ol start="4">
<li><p>깃허브에서 <strong>PR 날리기</strong></p>
</li>
<li><p><strong>코드리뷰 및 Approve</strong></p>
<p> PR의 Files changed 부분에서 코드와 함께 코멘트를 남길 수 있다.</p>
</li>
<li><p><strong>자신의 main 브랜치에 merge</strong></p>
<p> 모든 reviewer의 approve를 받고 main 브랜치에 각 주차의 브랜치를 병합. 이후 각 주차 브랜치는 삭제</p>
</li>
</ol>
<h2 id="advanced-skills">Advanced Skills</h2>
<h3 id="delete-push">Delete Push</h3>
<pre><code class="language-bash">git push -d {Remote Name} {Branch Name}</code></pre>
<p>원격에 브랜치를 푸시가 되어있는 상황에서, 푸시를 제거하고 싶을 때 수행하는 명령어</p>
<h3 id="fetch">Fetch</h3>
<p><code>pull</code>과 <code>fetch</code>의 가장 큰 차이점은 원격의 사항들을 가져와서 로컬에 자동으로 적용한다는 점
<code>git fetch</code>는 원격 저장소에서 변경 사항을 로컬로 가져오지만, work-tree에 반영하지는 않음
반면<code>git pull</code>은 원격 저장소에서 변경된 정보를 가져와 work-tree에 적용함</p>
<p>즉 <code>pull</code>은 <code>fetch</code> + <code>merge</code>를 동시에 수행하는 것임</p>
<br />
그렇다면 fetch를 왜 사용하는가?

<p>원격에 있는 변경사항을 로컬 Git에 적용하기 전 확인 가능 -&gt; merge 충돌에서 안전함</p>
<pre><code class="language-bash">git fetch    
git diff ...{원격 브랜치}</code></pre>
<h3 id="stash-임시저장">Stash (임시저장)</h3>
<p>: 현재 작업하던 브랜치에서 다른 브랜치로 전환하는데, 변경사항들을 임시저장하고 넘어가려고 할 때 사용</p>
<p>현재 적용된 commit이후로 변경된 모든 사항들이
stash 공간으로 이동된다.</p>
<pre><code class="language-bash">git stash</code></pre>
<pre><code class="language-bash">git stash push -m &quot;임시저장 타이틀&quot;</code></pre>
<pre><code class="language-bash">git stash list</code></pre>
<p>임시저장된 변경사항들의 리스트를 보여준다.</p>
<pre><code class="language-bash"># stash@{항목번호} 안쓰면 마지막 저장 항목 처리
git stash apply stash@{항목번호} # 임시저장된 항목 중 하나를 선택하여 적용
git stash drop stash@{항목번호} # 선택 항목 삭제
git stash pop stash@{항목번호} # 선택 항목 적용, 삭제</code></pre>
<p>치워둔 항목 처리</p>
<pre><code>git stash branch {브랜치명}</code></pre><p>새 브랜치를 생성해 pop - 충돌 예방</p>
<pre><code class="language-bash">git stash clear </code></pre>
<p>치워둔 모든 항목들 비우기    </p>
<h3 id="amend">Amend</h3>
<p>: 가장 최근의 커밋 메시지를 수정</p>
<pre><code class="language-bash">git commit --amend</code></pre>
<p>가장 최근 커밋 메시지를 수정하는 vi 모드로 진입한다.</p>
<pre><code class="language-bash">git commit --amend --no-edit</code></pre>
<p>커밋 메시지 수정 없이, 현재 stage한 항목을 이전 커밋에 넣는다.</p>
<h3 id="head">HEAD</h3>
<ul>
<li>HEAD는 일종의 포인터</li>
<li>터미널에서 현재 상태로 보여지는 값을 HEAD라고 한다.</li>
</ul>
<h3 id="checkout-into-hash">Checkout into Hash</h3>
<pre><code class="language-bash">git commit checkout {commit hash}</code></pre>
<p>특정 커밋 버전으로 돌아간다.</p>
<h3 id="merge-conflict-resolve-editor--gui-tools">Merge Conflict Resolve Editor &amp; GUI Tools</h3>
<ul>
<li><a href="https://www.gitkraken.com/">GitKraken</a></li>
<li>VSCode Extensions<ul>
<li><a href="https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens">GitLens</a></li>
<li><a href="https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github">GitHub Pull Requests</a></li>
</ul>
</li>
</ul>
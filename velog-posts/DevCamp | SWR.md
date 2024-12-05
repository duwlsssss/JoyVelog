<h3 id="swr">SWR</h3>
<ul>
<li>서버 상태 관리 역할(api와 밀접하게 관련됨)</li>
<li>특징<ul>
<li><strong>데이터 패칭(Fetching)</strong><ul>
<li>데이터를 서버에서 가져와서 사용 가능</li>
</ul>
</li>
<li><strong>캐싱(Caching)</strong><ul>
<li>가져온 데이터를 캐시에 저장하여 재요청 없이도 빠르게 가져올 수 있음</li>
</ul>
</li>
<li><strong>자동 리패칭(Auto Revalidation)</strong><ul>
<li>데이터가 만료되면 자동으로 최신 데이터를 가져옴</li>
</ul>
</li>
<li><strong>초기 로딩과 상태 관리</strong><ul>
<li>로딩 상태와 에러 상태를 자동으로 관리</li>
</ul>
</li>
<li><strong>집중적인 데이터 갱신(Deduplication)</strong><ul>
<li>같은 키로 요청이 중복되면 하나의 요청으로 합쳐 처리하여 성능 최적화</li>
</ul>
</li>
</ul>
</li>
</ul>
<ul>
<li>Tanstack Query vs SWR<ul>
<li>Tanstack Query<ul>
<li>cache key 필요, key에 대한 다양한 활용 가능</li>
<li>복잡한 패턴의 api 요청 및 관리에 대해 좀 더 수월히 처리 가능</li>
</ul>
</li>
<li>SWR<ul>
<li>명시적으로 key 입력을 받지 않음<ul>
<li>endpoint를 key로 사용할 수 있기 때문</li>
</ul>
</li>
<li>복잡한 데이터 관리 로직에 대해 설정이 힘듬</li>
</ul>
</li>
</ul>
</li>
</ul>
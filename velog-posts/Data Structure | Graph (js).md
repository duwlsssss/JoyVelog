<h2 id="그래프-graph">그래프 (Graph)</h2>
<p>: 정점(vertex)와 간선(edge)으로 나타냄
2가지 방식으로 구현</p>
<ul>
<li><strong>인접 행렬</strong>(adjacency matrix): 2차원 배열을 사용하는 방식<img src="https://velog.velcdn.com/images/kimlj0814/post/d993f5b8-3243-4174-b2b5-b3b332395f53/image.png" width="80%" />

</li>
</ul>
<p><strong>모든 정점들의 연결 여부를 저장해 O(V^2)의 메모리 필요 
하지만 두 노드의 연결 여부를 O(1)에 확인 가능</strong>
    -무방향 무가중치 그래프: 모든 간선이 방향성을 가지지 않고, 가중치도 없음
    <img src="https://velog.velcdn.com/images/kimlj0814/post/b5b6e51c-625b-478d-958a-ace014153ff7/image.png" width="80%" />
    -방향 가중치 그래프: 모든 간선이 방향, 가중치를 가지는 그래프
    <img src="https://velog.velcdn.com/images/kimlj0814/post/c3ba97d0-00b0-4c95-b629-44d12d32bfe5/image.png" width="80%" /></p>
<ul>
<li><strong>인접 리스트</strong>(adjacency list): 연결 리스트를 이용하는 방식</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/7931e94a-53ee-41ae-847f-2685e78604a0/image.png" /></p>
<p>-무방향 무가중치 그래프
    <img src="https://velog.velcdn.com/images/kimlj0814/post/6300badf-bbb5-47a2-bc66-c9233a5f8566/image.png" width="80%" /></p>
<p>-방향 가중치 그래프
    <img src="https://velog.velcdn.com/images/kimlj0814/post/aa878d09-2ae5-4fdf-995e-bf9348f3c45b/image.png" width="80%" /></p>
<p><strong>연결된 간선의 정보만 저장해 O(V+E)의 메모리 필요 
하지만 두 노드의 연결 여부를 확인하려면 O(V)의 시간 필요</strong></p>
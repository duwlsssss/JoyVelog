<h2 id="큐-queue">큐 (Queue)</h2>
<p>: 데이터 입출력이 FIFO(선입선출) 형태로 일어나는 자료구조</p>
<p>스택과 달리 입구와 출구가 다름
삽입이 일어나는 곳ㅡ후단
삭제가 일어나는 곳ㅡ전단</p>
<p>front는 큐의 첫번째 요소를 가리키고
rear는 마지막 요소를 가리킴</p>
<p>✅ 선형 큐
큐의 공간이 제한된 상태에서, 데이터를 삽입하고 삭제하면서 front와 tail 인덱스가 계속 증가함
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/2f9cfceb-9173-48c9-a20f-52dcc50fded2/image.png" /></p>
<ul>
<li>ADT<ul>
<li>객체: 두 가지 끝(front, rear)이 있으며, FIFO(First In, First Out) 규칙에 따라 동작하는 요소들의 모음</li>
<li>연산:
isEmpty(): 큐가 비어 있는지 확인 / 큐가 비어 있으면 true, 그렇지 않으면 false를 반환
enqueue(data): 큐의 rear에 요소를 추가하는 연산
dequeue(): 큐의 front에서 요소를 제거하고 반환하는 연산 / 큐가 비어 있으면 null을 반환
peek(): 큐의 front에 있는 요소를 확인하는 연산. 큐에서 제거하지는 않음 / 큐가 비어 있으면 null을 반환
getLength(): 큐에 있는 요소의 개수를 반환하는 연산
clear(): 큐를 초기화하여 모든 요소를 제거하는 연산
display(): 큐에 있는 모든 요소를 출력하는 연산. 디버깅 목적으로 사용</li>
</ul>
</li>
</ul>
<p>큐를 front, rear 포인터를 가진 연결리스트나, frontIndex,rearIndex 키를 가진 dictionary로 구현하면 삽입,삭제 연산이 O(1)의 시간 복잡도 가짐</p>
<p>+딕셔너리로 구현한 큐에서 frontIndex와 rearIndex 키를 사용하면 삭제(dequeue) 연산의 경우, 딕셔너리에서 요소를 삭제하는 것이 <strong>일반적으로 O(1)</strong>에 가까운 시간이 걸리지만, 메모리에서 실제로 해제되는 방식에 따라 구현 세부 사항에 따라 최악의 경우 <strong>O(n)</strong>이 될 수 있음.</p>
<ul>
<li>링크드 리스트<pre><code class="language-jsx">class Node {
constructor(data) {
  this.data = data;
  this.next = null;
}
}
</code></pre>
</li>
</ul>
<p>class Queue{
  constructor(){
    this.front=null;
    this.rear=null;
    this.size=0;
  }
  isEmpty(){
    return this.front === null; 
  }
  enqueue(data){
    const newNode = new Node(data);
    if(this.isEmpty()){
      this.front = newNode;
      this.rear = newNode;
    }
    this.rear.next = newNode;
    this.rear = newNode; 
    this.size++;
  }
  dequeue(){
    if(this.isEmpty()) return null; 
    const deaueueNode = this.front;
    this.front = this.front.next; 
    if(!this.front) this.rear = null; //요소 하나였을때 -&gt; 빈 큐가 됨
    this.size--;
    return deaueueNode.data;
  }
  peek(){
    if(this.isEmpty()) return null; 
    return this.front.data;
  }
  getLength(){
    return this.size;
  }
  clear(){
    this.front = null;
    this.rear = null;
    this.size = 0;
  }
  display(){
    if(this.isEmpty()){
      console.log('queue is empty!');
      return;
    }
    let current = this.front;
    while(current){
      console.log(current.data);
      current=current.next;
    }
  }
}</p>
<pre><code>- dictionary
```jsx
class Queue{
  constructor(){
    this.datas = {};
    this.frontIdx = 0;
    this.rearIdx = 0;
  }
  isEmpty(){
    return this.frontIdx === this.rearIdx; 
  }
  enqueue(data){
    this.datas[this.rearIdx] = data;
    this.rearIdx++;
  }
  dequeue(){
    if(this.isEmpty()) return null; 
    const data = this.datas[this.frontIdx];
    delete this.datas[this.frontIdx];
    this.frontIdx++;
    return data;
  }
  peek(){
    if(this.isEmpty()) return null; 
    return this.datas[this.frontIdx]
  }
  getLength(){
    return this.rearIdx-this.frontIdx;
  }
  display(){
    if(this.isEmpty()){
        console.log('queue is empty!');
        return;
      }
    else{
      for(let i = this.frontIdx; i&lt;this.rearIdx; i++){
        console.log(this.datas[i]);
      }
    }
  }
}</code></pre><pre><code class="language-jsx">// 출력 확인
const queue = new Queue();
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
queue.display(); // 10, 20, 30 출력
console.log(queue.dequeue()); // 10 반환 및 삭제
console.log(queue.peek());    // 20 출력
console.log(queue.getLength()); // 2 출력 (20, 30)
queue.display(); // 20, 30 출력</code></pre>
<p>✅ 원형 큐: 선형 큐와 달리 처음과 끝이 연결된 원형으로 공간을 재사용함
tail이 배열이나 리스트의 끝에 도달한 후에는 다시 처음으로 돌아가서 빈 공간을 채우며 삽입과 삭제가 일어남
-&gt; 고정된 크기의 큐에서 공간 낭비를 줄임
front, rear의 초기값이 0 / front는 항상 첫번째 요소의 하나 앞, rear는 마지막 요소를 가리킴
front==rear 이면 공백상태
front가 rear보다 하나 뒤에 있으면 포화상태</p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/9020e94f-b219-4666-bd94-4de0c5a512f3/image.png" /></p>
<p>✅ 우선 순위 큐(priority queue): 우선 순위에 따라 데이터를 추출하는 자료구조 </p>
<p>이진 트리 구조를 가짐 
리스트로 구현하면 삽입 O(1), 삭제 O(N)-우선 순위 순회 필요 
힙을 이용해 구현하면 삽입,삭제 O(1)</p>
<p>+heap: 원소들 중 <strong>최댓값, 최솟값을 빠르게 찾아내는 자료구조</strong>
완전 이진 트리 구조, 우선 순위가 높은 노드가 root에 위치
max heap: 값이 큰 원소부터 추출, 부모 노드 키값이 항상 자식 노드 키값보다 큼 -&gt; 루트 노드가 가장 큼, 값이 큰 데이터가 우선순위를 가짐
<img src="https://velog.velcdn.com/images/kimlj0814/post/2479b541-66d2-4c66-8e3f-8da6687dcabe/image.png" width="60%/" /></p>
<p>min heap: 값이 작은 원소부터 추출 / 부모 노드의 키 값이 자식 노드의 키값보다 항상 작음 -&gt; 루트 노드가 가장 작음, 값이 작은 데이터가 우선순위를 가짐</p>
<p>heapify: 최소 힙 구성 함수 / 부모로 올라가며, 부모보다 자신이 더 작으면 위치를 교체함 - 최소 힙 성질을 만족하도록
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/418e2837-ab65-4d83-bfe2-c3e3d6bac2be/image.png" /></p>
<p>원소 삽입: 부모로 거슬러 올라가며 부모보다 자신이 더 작으면 위치를 교체함
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/955e6945-e8f9-402f-a699-39b7cca5b7e1/image.png" /></p>
<p>원소 삭제: 마지막 노드가 루트 노드에 위치하게 한 후 루트노드에서 하향으로 heapify 진행해 작은 값을 적절히 위치하게 함
<img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/b3683037-2b55-4d4f-aa96-9694586d96b6/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/4f90c7a2-905d-4dc5-bf4b-9656cffdb0c0/image.png" /></p>
<p>-&gt; 거슬러 갈 때마다 처리해야 하는 범위에 포함된 원소 개수가 절반씩 줄어듦 -&gt; 삽입,삭제에 대한 시간 복잡도는 O(logN)</p>
<p>단순한 N개의 데이터를 힙에 넣었다 모두 꺼내는 작업은 O(NlogN)</p>
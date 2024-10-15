<h2 id="연결-리스트-linked-list">연결 리스트 (Linked List)</h2>
<p>연결리스트는 노드의 집합, <strong>노드</strong>는 데이터 필드+링크 필드(다른 노드의 주소값을 저장하는 포인터) 로 구성</p>
<p>포인터를 사용하면 하나의 자료에서 다음 자료로 쉽게 이동할 수 있음
연결리스트에선 첫 번째 노드를 알면 링크로 연결돼 있는 전체 노드에 모두 접근 가능
그래서 첫번째 노드를 가리키는 head 포인터 필요</p>
<ul>
<li>배열과 차이점
배열과 달리 요소들이 물리적으로 연속적인 메모리 주소나 인덱스에 저장되지 않음. 대신, <strong>각 요소(노드)</strong>는 다음 노드에 대한 참조를 가지므로 논리적으로 연결된 독립적인 객체임</li>
</ul>
<p>장점: 크기 제한 없이 필요할 때마다 동적으로 메모리 공간을 추가하여 사용할 수 있음(이 장점은 js에선 중요하지 않긴 함. 배열이 동적 배열로 구현돼있기 때문) / 중간 삽입 및 삭제(배열에서 중간에 요소를 삽입하거나 삭제하면 O(n)의 시간 복잡도를 가짐. 연결 리스트는 중간 노드에 접근하는 데만 시간이 걸리며, 삽입 및 삭제는 포인터 조작으로 처리되기 때문에 <strong>O(1)</strong> 시간 복잡도로 가능함)</p>
<img src="https://velog.velcdn.com/images/kimlj0814/post/55dce261-3c4e-42f2-9e79-d199aeaf7ef9/image.png" width="70%" />

<ul>
<li>단순 연결 리스트</li>
<li>ADT<ul>
<li>객체: 한방향으로 연결된 노드들의 리스트로, 각 노드는 데이터와 다음 노드에 대한 참조(포인터)를 포함한다.</li>
<li>연산:
getSize(): 리스트에 있는 노드의 개수를 반환하는 연산
clear(): 리스트의 모든 노드를 제거하는 연산
getFirst(): 리스트의 첫 번째 노드를 반환하는 연산 / 리스트가 비어 있을 경우 null 반환
getLast(): 리스트의 마지막 노드를 반환하는 연산 / 리스트가 비어 있을 경우 null 반환
insertFirst(data): 리스트의 가장 앞에 새로운 노드를 삽입하는 연산
insertLast(data): 리스트의 마지막에 새로운 노드를 삽입하는 연산<br />insertAt(data, index): 리스트의 지정된 위치에 새로운 노드를 삽입하는 연산 / 인덱스가 유효하지 않으면 "Index out of bounds" 메시지 출력
removeFirst(): 리스트의 첫 번째 노드를 삭제하고 그 데이터를 반환하는 연산 / 리스트가 비어 있을 경우 null
removeLast(): 리스트의 마지막 노드를 삭제하고 그 데이터를 반환하는 연산 / 리스트가 비어 있을 경우 null
removeAt(index): 리스트의 지정된 위치에 있는 노드를 삭제하고 그 데이터를 반환하는 연산 / 인덱스가 유효하지 않을 경우 "Index out of bounds" 메시지 출력
display(): 리스트의 모든 노드의 데이터를 순차적으로 출력하는 연산 / 리스트가 비어 있을 경우 "linkedList is empty!" 메시지 출력</li>
</ul>
</li>
</ul>
<pre><code class="language-jsx">class Node { // 독립적인 노드 객체
  constructor(data) {
    this.data = data; // 데이터 필드
    this.next = null; // 링크 필드
  }
}

class LinkedList {
  constructor(head = null) {
      this.head = head
  }
  getSize(){
    let cnt = 0;
    let node = this.head;
    while(node){
      cnt++;
      node = node.next;
    }
    return cnt;
  } 
  clear(){
    this.head=null;
  }
  getFirst(){
    if(!this.head){return null;}
    return this.head;
  }
  getLast(){
    if(!this.head){return null;}
    let lastNode = this.head;
    while(lastNode.next){
      lastNode = lastNode.next
    }
    return lastNode;
  }
  insertFirst(data){
    const nNode = new Node(data);
    nNode.next = this.head;
    this.head = nNode;
  }
  inertLast(data){
    const nNode = new Node(data);
    if(!this.head){this.head=nNode;} // 빈 리스트면
    else{
      const lastNode = this.getLast();
      lastNode.next = nNode;
    }
  }
  insertAt(data,index){
    if(index===0) return this.insertFirst(data);
    const nNode = new Node(data);
    let prev = null;
    let current = this.head;
    let cnt = 0;
    while(current&amp;&amp;cnt&lt;index){
      prev = current;
      current = current.next
      cnt++;
    }
    if(!current &amp;&amp; cnt !== index){ // current가 null이면 새로운 노드를 삽입하려는 위치가 리스트의 마지막일 수 있음. 이 경우는 제외하고 메시지 표시
      console.log("Index out of bounds");
      return; 
    }
    prev.next = nNode;
    nNode.next = current;
  }
  removeFirst(){
    if(!this.head) return null;
    const removeNode = this.head;
    this.head = this.head.next; 
    return removeNode.data;
  }
  removeLast(){
    if(!this.head) return null;
    if(!this.head.next){ // 요소 1개일 때
      const removeNode = this.head;
      this.head = null; 
      return removeNode.data;
    }
    let prev = null;
    let current = this.head;
    while(current.next){ // current.next가 있을때까지, 즉 prev가 삭제할 노드 바로 전 노드가 되게
      prev = current;
      current = current.next;
    }
    prev.next = null;
    return current.data;
  }
  removeAt(index){
    if(index===0) return this.removeFirst();
    let prev = null;
    let current = this.head;
    let cnt = 0;
    while(current&amp;&amp;cnt&lt;index){
      prev = current;
      current = current.next;
      cnt++;
    }
    if(current){
      prev.next = current.next;
      return current.data;
    }
    console.log("Index out of bounds");
    return null; // 해당 인덱스의 요소가 없음 
  }
  //디버깅용
  display(){
    if(!this.head) {
      console.log('linkedList is empty!');
      return;
    }
    let current = this.head;
    while(current){
      console.log(current.data);
      current = current.next;
    }
  }
}</code></pre>
<pre><code class="language-jsx">// 출력 확인
const ll = new LinkedList();
ll.insertFirst(10); // 첫 번째에 10 삽입 ( head -&gt; [10] -&gt; null)
ll.insertFirst(5);  // 첫 번째에 5 삽입 (head -&gt; [5] -&gt; [10] -&gt; null)
ll.insertAt(15, 1); // 인덱스 1에 15 삽입 (head -&gt; [5] -&gt; [15] -&gt; [10] -&gt; null)
console.log(ll.getSize()); // 3 출력
ll.display(); // 5, 15, 10 출력</code></pre>
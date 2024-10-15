<h2 id="스택-stack">스택 (Stack)</h2>
<p>: 데이터 입출력이 LIFO(후입선출) 형태로 일어나는 자료구조
<img src="https://velog.velcdn.com/images/kimlj0814/post/6cd1691d-3b98-40d0-817e-2b4cd5f1be2d/image.png" width="50%" /></p>
<p>top 포인터가 스택의 가장 마지막 요소를 가리킴</p>
<ul>
<li>ADT:<ul>
<li>객체: LIFO(Last In, First Out) 규칙에 따라 동작하는 요소들의 모음</li>
<li>연산:
  isEmpty(): 스택이 비어 있는지 확인 / 스택이 비어 있으면 true, 그렇지 않으면 false를 반환
  push(data): 스택의 top에 요소를 추가하는 연산
  pop(): 스택의 top에 있는 요소를 제거하고 반환하는 연산 / 스택이 비어 있으면 null을 반환
  peek(): 스택의 top에 있는 요소를 확인하는 연산. 제거하지는 않음 / 스택이 비어 있으면 null을 반환
  getSize(): 스택에 있는 요소의 개수를 반환하는 연산
  clear(): 스택을 초기화하여 모든 요소를 제거하는 연산
  display(): 스택의 모든 요소를 출력하는 연산. 디버깅 목적으로 사용</li>
</ul>
</li>
</ul>
<p>스택을 배열로 만들어 마지막 인덱스만 처리하거나, top을 가리키는 하나의 포인터만 만들어(마지막 요소 가리킴) 연결리스트로 만들면 삽입,삭제 연산이 O(1)의 시간 복잡도 가짐 </p>
<ul>
<li><p>배열</p>
<pre><code class="language-jsx">class Stack{
constructor(){
  this.datas = [] ;
}
push(data) {
  this.datas.push(data); 
}
pop() {
  if(this.isEmpty()) return null;
  return this.datas.pop(); 
}
peek() {
  if (this.isEmpty()) return null; 
  return this.datas[this.getSize() - 1]; 
}
getSize(){
  return this.datas.length;
}
isEmpty() {
  return this.getSize() === 0;
}
display() {
  if (this.isEmpty()) {
    console.log('Stack is empty!');
    return;
  }
  for(let i=this.getSize()-1; i&gt;=0; i--){
    console.log(this.datas[i]);
  }
}
}</code></pre>
</li>
<li><p>링크드리스트</p>
<pre><code class="language-jsx">class Node {
constructor(data) {
  this.data = data;
  this.next = null;
}
}
</code></pre>
</li>
</ul>
<p>class Stack{
  constructor(){
    this.top=null;
    this.size=0;
  }
  push(data){
    const nNode = new Node(data);
    nNode.next = this.top;
    this.top = nNode;
    this.size++;
  }
  pop(){
    if(this.isEmpty()) return null; // 빈 스택이면 null 반환
    const popNode = this.top;
    this.top = this.top.next;
    this.size--;
    return popNode.data;
  }
  peek(){
    if(this.isEmpty()) return null;
    return this.top.data;
  }
  isEmpty(){
    return this.top===null;
  }
  getSize(){
    return this.size;
  }
  display(){
    if(this.isEmpty()){
      console.log('queue is empty!');
      return;
    }
    let current = this.top;
    while(current){
      console.log(current.data);
      current=current.next;
    }
  }
}</p>
<pre><code>```jsx
// 출력 확인
const stack = new Stack();
console.log(stack.isEmpty()); // true 출력
stack.push(10);
stack.push(20);
stack.push(30);
stack.display(); // 30, 20, 10 출력
console.log(stack.pop()); // 30 반환 및 삭제
console.log(stack.peek());    // 20 출력
console.log(stack.getSize()); // 2 (20, 10)
stack.display(); // 20, 10 출력</code></pre>
# Data Structure
## 💡 각종 자료구조 (ex. LinkedList, Queue, Stack, Tree, ...) 를 구현해 보세요.
### 1. Linear Data Structure
**1) Array**
  ```python
  # 배열 선언 및 초기화
  arr = [1, 2, 3, 4, 5]
  
  # 배열 요소 접근
  print(arr[0])  # 1
  
  # 배열 길이
  print(len(arr))  # 5
  
  # 배열 요소 변경
  arr[0] = 10
  
  # 배열 순회
  for element in arr:
      print(element)
  ```
</br>

**2) Stack**   
  ```python
  # 배열(array)를 이용해 구현 - python 리스트와 메서드들을 이용
  class ArrayStack:
      def __init__(self): # 빈 스택을 초기화
          self.data = []
          
      def size(self): # 스택의 크기를 리턴
          return len(self.data)
      
      def isEmpty(self): # 스택이 비어 있는지 판단
          return self.size() == 0
      
      def push(self, item): # 데이터 원소를 추가
          self.data.append(item)
          
      def pop(self): # 데이터 원소를 삭제(리턴)
          return self.data.pop()
      
      def peek(self): # 스택의 꼭대기 원소 반환
          return self.data[-1]
  ```
<details>
  <summary>추상적 자료구조 구현 표현</summary>
  
```python
# 연결 리스트(linked list)를 이용해 구현 - 양방향 연결 리스트 이용
class Node:
    
    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None

class DoublyLinkedList:
    
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None
    
    # 리스트 출력
    def __repr__(self):
        if self.nodeCount == 0:
            return 'LinkedList: empty'
        
        s = ''
        curr = self.head
        while curr.next.next:
            curr = curr.next
            s += repr(curr.data)
            if curr.next.next is not None:
                s += ' -> '
        return s
    
    # 길이 리턴
    def getLength(self):
        return self.nodeCount
    
    # 리스트 순회
    def traverse(self):
        result = []
        curr = self.head
        while curr.next.next:
            curr = curr.next
            result.append(curr.data)
        return result
    
    # 리스트 역순회
    def reverse(self):
        result = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            result.append(curr.data)
        return result
    
    # 특정 원소 참조
    def getAt(self, pos): # getAt(0) -> head
        if pos < 0 or pos > self.nodeCount:
            return None
        
        if pos > self.nodeCount // 2:
            i = 0
            curr = self.tail
            while i < self.nodeCount - pos + 1:
                curr = curr.prev
                i += 1
        else:
            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1
                
        return curr
    
    # 원소의 삽입
    def insertAfter(self, prev, newNode): # prev가 가리키는 node의 다음에 newNode를 삽입하고 성공/실패에 따라 True/False를 리턴
        next = prev.next
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True
    
    def insertBefore(self, next, newNode):
        prev = next.prev
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True
    
    def insertAt(self, pos, newNode):
        if pos < 1 or pos > self.nodeCount + 1:
            return False
        prev = self.getAt(pos-1) # newNode가 삽입될 위치
        return self.insertAfter(prev, newNode)
    
    # 원소의 삭제
    def popAfter(self, prev): # prev의 다음 node를 삭제하고, 그 node의 data를 리턴
        curr = prev.next
        next = curr.next
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data
    
    def popBefore(self, next): # next의 이전에 있던 node를 삭제하고, 그 node의 data를 리턴
        curr = next.prev
        prev = curr.prev
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data
    
    def popAt(self, pos): # pos에 의해 지정되는 node를 삭제하고, 그 node의 data를 리턴
        if pos < 1 or pos > self.nodeCount:
            raise IndexError
        prev = self.getAt(pos-1)
        return self.popAfter(prev)
        
    def concat(self, L):
        self.tail.prev.next = L.head.next
        L.head.next.prev = self.tail.prev
        self.tail = L.tail
        self.nodeCount += L.nodeCount

class LinkedListStack:

    def __init__(self):
        self.data = DoublyLinkedList()

    def size(self): # 스택의 크기를 리턴
        return self.data.getLength()

    def isEmpty(self): # 비어 있으면 True, 원소가 있으면 False 리턴
        return self.size() == 0 

    def push(self, item): # 데이터 원소를 추가
        node = Node(item)
        self.data.insertAt(self.size() + 1, node) # 맨 뒤 노드에 추가

    def pop(self): # 데이터 원소를 삭제(리턴)
        return self.data.popAt(self.size()) # 맨 뒤 노드(Top)를 리턴

    def peek(self): # 스택의 꼭대기 원소 반환
        return self.data.getAt(self.size()).data # 맨 뒤 노드(Top) 반환
```
</details>
</br>

**3) Queue**
```python
# 배열(array)을 이용해 구현 - python 리스트와 메서들을 이용
class ArrayQueue:
    
    def __init__(self): # 빈 큐를 초기화
        self.data = []
        
    def size(self): # 큐의 크기를 리턴 - O(1)
        return len(self.data)
    
    def isEmpty(self): # 큐가 비어 있는지 판단 - O(1)
        return self.size() == 0
    
    def enqueue(self, item): # 데이터 원소를 추가 - O(1)
        self.data.append(item)
        
    def dequeue(self): # 데이터 원소를 삭제(리턴) - O(N)
        return self.data.pop(0) 
    
    def peek(self): # 큐의 맨 앞 원소 반환 - O(1)
        return self.data[0]
```

<details>
  <summary>추상적 자료구조 구현 표현</summary>
  
```python
# 연결 리스트(linked list)를 이용해 구현 - 양방향 연결 리스트 이용
class Node:

    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None


class DoublyLinkedList:

    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None


    def __repr__(self):
        if self.nodeCount == 0:
            return 'LinkedList: empty'

        s = ''
        curr = self.head
        while curr.next.next:
            curr = curr.next
            s += repr(curr.data)
            if curr.next.next is not None:
                s += ' -> '
        return s


    def getLength(self):
        return self.nodeCount


    def traverse(self):
        result = []
        curr = self.head
        while curr.next.next:
            curr = curr.next
            result.append(curr.data)
        return result


    def reverse(self):
        result = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            result.append(curr.data)
        return result


    def getAt(self, pos):
        if pos < 0 or pos > self.nodeCount:
            return None

        if pos > self.nodeCount // 2:
            i = 0
            curr = self.tail
            while i < self.nodeCount - pos + 1:
                curr = curr.prev
                i += 1
        else:
            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1

        return curr


    def insertAfter(self, prev, newNode):
        next = prev.next
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True


    def insertAt(self, pos, newNode):
        if pos < 1 or pos > self.nodeCount + 1:
            return False

        prev = self.getAt(pos - 1)
        return self.insertAfter(prev, newNode)


    def popAfter(self, prev):
        curr = prev.next
        next = curr.next
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data


    def popAt(self, pos):
        if pos < 1 or pos > self.nodeCount:
            raise IndexError('Index out of range')

        prev = self.getAt(pos - 1)
        return self.popAfter(prev)


    def concat(self, L):
        self.tail.prev.next = L.head.next
        L.head.next.prev = self.tail.prev
        self.tail = L.tail

        self.nodeCount += L.nodeCount


class LinkedListQueue:

    def __init__(self):
        self.data = DoublyLinkedList()

    def size(self):
        return self.data.nodeCount

    def isEmpty(self):
        return self.data.nodeCount==0

    def enqueue(self, item):
        node = Node(item)
        self.data.insertAt(self.size()+1,node)

    def dequeue(self):
        return self.data.popAt(1)

    def peek(self):
        return self.data.head.next.data
```
</details>

<details>
  <summary> Circular Queue </summary>
  
```python
MAX_QSIZE = 10
class CircularQueue :
    def __init__(self):
        self.front = 0
        self.rear = 0
        self.items = [None] * MAX_QSIZE

    def isEmpty(self):
        return self.front == self.rear

    def isFull(self):
        return self.front == (self.rear+1)%MAX_QSIZE

    def clear(self):
        self.front = self.rear
    
    def __len__(self):
        return (self.rear - self.front + MAX_QSIZE) % MAX_QSIZE
    
    def enqueue(self, item):
        if not self.isFull():
            self.rear = (self.rear + 1) % MAX_QSIZE
            self.items[self.rear] = item
    
    def dequeue(self):
        if not self.isEmpty():
            self.front = (self.front+1) % MAX_QSIZE
            return self.items[self.front]


    def peek(self):
        if not self.isEmpty():
            return self.items[(self.front+1)%MAX_QSIZE]
    
    def print(self):
        out = []
        if self.front < self.rear:
            out = self.items[self.front+1:self.rear+1]
        else:
            out = self.items[self.front+1:MAX_QSIZE] + self.items[0:self.rear+1]

        print("[f=%s, r=%d] ==> "%(self.front, self.rear), out)

class CircularDeque(CircularQueue):
    
    def __init__(self):
        super().__init__()

    def addRear (self, item):
        self.enqueue(item)

    def deleteFront (self):
        return self.dequeue()
    
    def getFront(self):
        return self.peek()

    def addFront(self, item):
        if not self.isFull():
            self.items[self.front] = item
            self.front = (self.front - 1 + MAX_QSIZE) % MAX_QSIZE

    def deleteRear(self):
        if not self.isEmpty():
            item = self.items[self.rear]
            self.rear = (self.rear - 1 + MAX_QSIZE) % MAX_QSIZE
            return item

    def getRear(self):
        return self.items[self.rear]
```
</details>
<details>
  <summary> Priority Queue </summary>

```python
import heapq

# 우선순위 큐 생성
priority_queue = []

# 요소 추가 (우선순위와 값의 튜플로 추가)
heapq.heappush(priority_queue, (3, "Apple"))
heapq.heappush(priority_queue, (1, "Banana"))
heapq.heappush(priority_queue, (2, "Cherry"))

# 가장 높은 우선순위 요소 확인
highest_priority = priority_queue[0]
print("Highest Priority:", highest_priority[1])

# 우선순위 큐에서 요소 추출
while priority_queue:
    priority, value = heapq.heappop(priority_queue)
    print(f"Priority: {priority}, Value: {value}")
```
</details>
</br>

**4) Deque**
```python
# 배열(array)를 이용해 구현 - python 리스트와 메서드들을 이용
# head: 가장 앞에 있는 원소의 인덱스
# tail: 가장 뒤에 있는 원소의 인덱스 + 1
# head와 tail의 초기값은 MX, 배열의 크기: 2*MX+1
# -> 덱은 스택, 큐와 달리 양쪽에서 삽입이 가능하기 때문에 양쪽으로 확장하려면 시작점을 배열의 중간으로 둬야 한다.

class ArrayDeque:
    def __init__(self): # 빈 덱 초기화
        MX = 1000005 # 배열의 중간
        self.dat = [False] * (2*MX+1) # 임의로 설정한 배열의 크기
        self.head = MX
        self.tail = MX
        
    def push_front(self, x): # 원소 x를 덱의 앞에 추가
        self.dat[self.head-1] = x
        self.head -= 1

    def push_back(self, x): # 원소 x를 덱의 뒤에 추가
        self.dat[self.tail] = x
        self.tail += 1

    def pop_front(self): # 덱의 가장 앞에 있는 원소를 제거, 반환
        if self.head == self.tail: # 덱이 비어있음
            return -1
        self.head += 1
        return dat[self.head-1] # 여기서 굳이 제거해 줄 필요가 없는 이유는 이후에 다른 원소로 채워지게 되면 덮어씌우면 되기 때문

    def pop_back(self): # 덱의 가장 뒤에 있는 원소를 제거, 반환
        if self.head == self.tail:
            return -1
        self.tail -= 1
        return dat[self.tail+1]
    
    def size(self): # 덱의 원소의 개수를 반환
        return self.tail - self.head

    def isEmpty(self): # 덱이 비어있으면 1, 아니면 0
        if self.head == self.tail:
            return 1
        else:
            return 0
    
    def front(self): # 덱의 가장 앞에 있는 원소를 반환
        return self.dat[self.head]

    def back(self): # 덱의 가장 뒤에 있는 원소를 반환
        return self.dat[self.tail-1]
```
</br>
     
**5) Linked List**
```python
class Node:
    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None

class LinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = None
        self.head.next = self.tail
    
    # 리스트 출력
    def __repr__(self):
        if self.nodeCount == 0:
            return 'LinkedList: empty'
        s = ''
        curr = self.head
        while curr is not None:
            s += repr(curr.data)
            if curr.next is not None:
                s += ' -> '
            curr = curr.next
        return s
    
    # 길이 리턴
    def getLength(self):
        return self.nodeCount
    
    # 리스트 순회
    def traverse(self):
        result = []
        curr = self.head
        while curr.next:
            curr = curr.next
            result.append(curr.data)
        return result
    
    # 특정 원소 참조
    def getAt(self, pos): # getAt(0) -> head
        if pos < 0 or pos > self.nodeCount:
            return None
        i = 0
        curr = self.head
        while i < pos:
            curr = curr.next
            i += 1
        return curr
    
    # 원소의 삽입
    def insertAfter(self, prev, newNode): # prev가 가리키는 node의 다음에 newNode를 삽입하고 성공/실패에 따라 True/False를 리턴
        newNode.next = prev.next
        if prev.next is None: # tail
            self.tail = newNode
        prev.next = newNode
        self.nodeCount += 1
        return True
    
    def insertAt(self, pos, newNode): # 1 <= pos <= nodeCount+1, pos가 가리키는 위치에 newNode를 삽입하고, 성공/실패에 따라 True/False를 리턴
        # (1) pos범위 조건 확인
        # (2) pos == 1인 경우에는 head뒤에 새 node 삽입
        # (3) pos == nodeCount + 1인 경우는 prev <- tail
        # (4) 그렇지 않은 경우에는 prev <- getAt(…)
        if pos < 1 or pos > self.nodeCount + 1:
            return False
        if pos != 1 and pos == self.nodeCount + 1: # pos가 1이면서 nodeCount+1과 같으면 빈 리스트에 노드를 삽입하는 경우
            prev = self.tail
        else: 
            prev = self.getAt(pos-1) # newNode가 삽입될 위치
        return self.insertAfter(prev, newNode)
    
    # 원소의 삭제
    def popAfter(self, prev): # prev의 다음 node를 삭제하고, 그 node의 data를 리턴
        if prev: # prev가 마지막 node일 때
            return None
        curr = prev.next
        prev.next = curr.next
        if curr.next is None:
            self.tail = prev
        self.nodeCount -= 1
        return curr.data
    
    def popAt(self, pos):
        if pos < 1 or pos > self.nodeCount:
            raise IndexError
        prev = self.getAt(pos-1)
        return self.popAfter(prev)
        
    def concat(self, L):
        self.tail.next = L.head.next
        if L.tail:
            self.tail = L.tail
        self.nodeCount += L.nodeCount
```
</br>

**6) Linked List**
```python
class Node:
    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None
    
    # 리스트 출력
    def __repr__(self):
        if self.nodeCount == 0:
            return 'LinkedList: empty'
        s = ''
        curr = self.head
        while curr.next.next:
            curr = curr.next
            s += repr(curr.data)
            if curr.next.next is not None:
                s += ' -> '
        return s
    
    # 길이 리턴
    def getLength(self):
        return self.nodeCount
    
    # 리스트 순회
    def traverse(self):
        result = []
        curr = self.head
        while curr.next.next:
            curr = curr.next
            result.append(curr.data)
        return result
    
    # 리스트 역순회
    def reverse(self):
        result = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            result.append(curr.data)
        return result
    
    # 특정 원소 참조
    def getAt(self, pos): # getAt(0) -> head
        if pos < 0 or pos > self.nodeCount:
            return None
        
        if pos > self.nodeCount // 2:
            i = 0
            curr = self.tail
            while i < self.nodeCount - pos + 1:
                curr = curr.prev
                i += 1
        else:
            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1
        return curr
    
    # 원소의 삽입
    def insertAfter(self, prev, newNode): # prev가 가리키는 node의 다음에 newNode를 삽입하고 성공/실패에 따라 True/False를 리턴
        next = prev.next
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True
    
    def insertBefore(self, next, newNode):
        prev = next.prev
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True
    
    def insertAt(self, pos, newNode):
        if pos < 1 or pos > self.nodeCount + 1:
            return False
        prev = self.getAt(pos-1) # newNode가 삽입될 위치
        return self.insertAfter(prev, newNode)
    
    # 원소의 삭제
    def popAfter(self, prev): # prev의 다음 node를 삭제하고, 그 node의 data를 리턴
        curr = prev.next
        next = curr.next
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data
    
    def popBefore(self, next): # next의 이전에 있던 node를 삭제하고, 그 node의 data를 리턴
        curr = next.prev
        prev = curr.prev
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data
    
    def popAt(self, pos): # pos에 의해 지정되는 node를 삭제하고, 그 node의 data를 리턴
        if pos < 1 or pos > self.nodeCount:
            raise IndexError
        prev = self.getAt(pos-1)
        return self.popAfter(prev)
        
    def concat(self, L):
        self.nodeCount += L.nodeCount
        self.tail.prev.next = L.head.next
        L.head.next.prev = self.tail.prev
        self.tail = L.tail
```
</br>

**7) Hash Table**

```python
# 해시 테이블 생성
hash_table = {}

# 데이터 추가
hash_table["Alice"] = 25
hash_table["Bob"] = 30

# 데이터 조회
age = hash_table.get("Alice")

# 해시 충돌 처리를 위한 체이닝 (Chaining) 사용
hash_table["Charlie"] = [35, "New York"]
```
</br>

### 2. NonLinear Data Structure
**1) Tree**
```python
class BinaryTree:
    def __init__(self, array):
        self.array = array

    def find_children(self):
        """
        자식 노드 탐색
        """
        i = 0
        n = len(self.array)
        while i < n:
            if self.array[i]:
                print(f"Parent: {self.array[i]}", end = ", ")
                left = 2 * i + 1
                right = left + 1
                if left < n and self.array[left]:
                    print(f"Left: {self.array[left]}", end = ", ")
                if right < n and self.array[right]:
                    print(f"Right: {self.array[right]}", end = " ")
                print()
            i+=1

    def find_parents(self):
        """
        부모 노드 탐색
        """
        n = len(self.array)
        i = n-1

        while i > 0:
            if self.array[i]:
                print(f"Parent of {self.array[i]} -> {self.array[(i-1)//2]}")
            i-=1

    def get_preorder_result(self):
        """
        전위 순회
        """
        def preorder(idx=0):
            if idx < len(self.array):
                result.append(self.array[idx])
                left = 2 * idx + 1
                right = left + 1

                if left < len(self.array) and self.array[left]:
                    preorder(left)
                if right < len(self.array) and self.array[right]:
                    preorder(right)
        
        result = []
        preorder()
        return result
    
    def get_inorder_result(self):
        """
        중위 순회
        """
        def inorder(idx=0):
            if idx < len(self.array):
                left = 2 * idx + 1
                right = left + 1

                if left < len(self.array) and self.array[left]:
                    inorder(left)
                result.append(self.array[idx])
                if right < len(self.array) and self.array[right]:
                    inorder(right)
        
        result = []
        inorder()
        return result
    
    def get_postorder_result(self):
        """
        후외 순회
        """
        def postorder(idx=0):
            if idx < len(self.array):
                left = 2 * idx + 1
                right = left + 1

                if left < len(self.array) and self.array[left]:
                    postorder(left)
                if right < len(self.array) and self.array[right]:
                    postorder(right)

                result.append(self.array[idx])
        
        result = []
        postorder()
        return result
```
```python
class Node(object):
    def __init__(self, data):
        self.data = data
        self.left = self.right = None
                
class BinarySearchTree(object):
    
    def __init__(self):
        self.root = None
        
    def insert(self, data):
        self.root = self._insert_value(self.root, data)
        return self.root is not None

    def _insert_value(self, node, data):
        if node is None:
            node = Node(data)
        else:
            if data <= node.data:
                node.left = self._insert_value(node.left, data)
            else:
                node.right = self._insert_value(node.right, data)
        return node
        
    def find(self, key):
        return self._find_value(self.root, key)

    def _find_value(self, root, key):
        if root is None or root.data == key:
            return root is not None
        elif key < root.data:
            return self._find_value(root.left, key)
        else:
            return self._find_value(root.right, key)
    
    def delete(self, key):
        self.root, deleted = self._delete_value(self.root, key)
        return deleted # 삭제 여부

    def _delete_value(self, node, key):
        if node is None:
            return node, False

        deleted = False # 삭제 여부
        if key == node.data: # 삭제할 노드를 찾으면
            deleted = True # 삭제 가능
            if node.left and node.right: # 삭제할 노드가 왼쪽, 오른쪽 자식 노드를 가지고 있을 경우
                # replace the node to the leftmost of node.right
                parent, child = node, node.right
                while child.left is not None: # 삭제할 노드의 오른쪽 노드 중 왼쪽 노드의 가장 작은 값
                    parent, child = child, child.left
                child.left = node.left
                if parent != node:
                    parent.left = child.right
                    child.right = node.right
                node = child
            elif node.left or node.right: # 삭제할 노드가 왼쪽 or 오른쪽 노드 중 하나를 가지고 있는 경우
                node = node.left or node.right
            else:
                node = None
        elif key < node.data:
            node.left, deleted = self._delete_value(node.left, key)
        else:
            node.right, deleted = self._delete_value(node.right, key)
        return node, deleted
```
</br>

**2) Heap**
```python
class Heap:
    def __init__(self, heap=[]):
        self.heap = heap
        self.heapify()

    def heapify(self):
        last = len(self.heap) // 2 - 1  # 가장 마지막 서브트리
        for current in range(last, -1, -1):
            while current <= last:
                child = current * 2 + 1 # 왼쪽 자식
                sibling = child + 1 # 오른쪽 자식
                if sibling < len(self.heap) and self.heap[child] > self.heap[sibling]:  # 더 작은 자식 선택
                    child = sibling
                if self.heap[current] > self.heap[child]:   # 현재(부모) 가 자식보다 크면
                    self.heap[current], self.heap[child] = self.heap[child], self.heap[current] # 교환
                    current = child # 위치 업데이트
                else:
                    break

    def heappush(self, data):
        self.heap.append(data)   
        current = len(self.heap)-1   # 추가한 원소의 인덱스
        while current > 0:  # 루트에 도달하면 종료
            parent = (current-1)//2 # 현재 위치의 부모 노드 찾기
            if self.heap[parent] > self.heap[current]:    # 부모노드가 더 크면
                self.heap[parent], self.heap[current] = self.heap[current], self.heap[parent]   # 교환
                current=parent  # 현재 위치 업데이트
            else:   # 아니면
                break   # 제자리 찾아감

    def heappop(self):
        if not self.heap:
            raise IndexError('empty heap')
        
        root, self.heap[0] = self.heap[0], self.heap.pop()   # 루트 노드 임시 저장, 마지막 노드를 루트로 옮김
        current, child = 0, 1   # 현재 노드의 인덱스와 왼쪽 자식 인덱스

        while child < len(self.heap):
            sibling = child+1   # 오른쪽 자식 인덱스
            if sibling < len(self.heap) and self.heap[child] > self.heap[sibling]:  # 비교할 자식 노드 선택 (더 작은 값)
                child = sibling
            if self.heap[current] > self.heap[child]:   # 더 작은 자식 노드보다 현재 노드가 더 크면
                self.heap[current], self.heap[child] = self.heap[child], self.heap[current] # 해당 자식과 현재 노드 교환
                current = child     # 현재 인덱스 업데이트
                child = current * 2 + 1     # 자식 인덱스 업데이트
            else:
                break

        return root


    def __str__(self):
        return ' '.join(map(str, self.heap))
```


</br>

## 📑 꼬리질문
### 그래프 표현 방식
**1) 인접 행렬**
```python
class Graph:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.adj_matrix = [[0] * num_vertices for _ in range(num_vertices)]

    def add_edge(self, start, end):
        if 0 <= start < self.num_vertices and 0 <= end < self.num_vertices:
            self.adj_matrix[start][end] = 1
            self.adj_matrix[end][start] = 1  # 무방향 그래프라면 이 줄은 생략

    def print_adj_matrix(self):
        for row in self.adj_matrix:
            print(" ".join(map(str, row)))

# 그래프 생성
num_vertices = 5
graph = Graph(num_vertices)

# 간선 추가
graph.add_edge(0, 1)
graph.add_edge(0, 4)
graph.add_edge(1, 2)
graph.add_edge(1, 3)
graph.add_edge(1, 4)
graph.add_edge(2, 3)
graph.add_edge(3, 4)

# 인접 행렬 출력
graph.print_adj_matrix()
```
</br>

**2) 인접 리스트**
```python
class Graph:
    def __init__(self):
        self.adj_list = {}

    def add_edge(self, start, end):
        if start in self.adj_list:
            self.adj_list[start].append(end)
        else:
            self.adj_list[start] = [end]

        if end in self.adj_list:
            self.adj_list[end].append(start)
        else:
            self.adj_list[end] = [start]

    def print_adj_list(self):
        for vertex, neighbors in self.adj_list.items():
            print(f"{vertex}: {' -> '.join(map(str, neighbors))}")

# 그래프 생성
graph = Graph()

# 간선 추가
graph.add_edge(0, 1)
graph.add_edge(0, 4)
graph.add_edge(1, 2)
graph.add_edge(1, 3)
graph.add_edge(1, 4)
graph.add_edge(2, 3)
graph.add_edge(3, 4)

# 인접 리스트 출력
graph.print_adj_list()
```
</br>
</br>

## 📚 Reference
[깃허브 - 자료구조/알고리즘 구현 코드 정리](https://github.com/ksumini/Algorithm-Practice/tree/main/Note)

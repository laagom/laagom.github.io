---
layout: post
title:  "[Language] 연결리스트(Linked List) in Python" 
subtitle:   "연결리스트(Linked List) in Python"
date: 2023-03-14
categories: study
tags: python
comments: true
image: /assets/img/lagom.png
related_posts:
    - /_posts/study/computer_science/2023-01-31-중앙처리장치(CPU) 작동 원리.md
    - /_posts/study/computer_science/2023-01-31-중앙처리장치(CPU) 작동 원리.md
---

# 연결 리스트(Linked List) in Python
연결 리스트(Linked List)는 `(데이터와 다음 노드의 주소를 담고 있는)노드`들이 한 줄로 연결되어 있는 방식의 자료구조이다.

![Alt text](/assets/resources/linked%20list_node-001.jpeg)
연결되는 방향에 따라 `단일 연결 리스트(Singly Linked List)`, `이중 연결 리스트(Doubly Linked List)`, `환형 연결 리스트(Circular Linked List)` 세 가지가 존재한다.

<br>

## 연결 리스트(Linked list)란?
연결 리스트(Linked List)는 데이터를 노드의 형태로 저장한다. 노드에는 `데이터(값)`와 `다음 노드를 가르키는 포인터`를 담은 구조로 이루어져 있다.

![Alt text](/assets/resources/linked%20list_node-002.jpeg)

연결 리스트(Linked List는 Array)는 배열(Array) 처럼 `선형 데이터 자료구조`이지만, 배열(Array)는 물리적인 배치 구조 자체가 연속적으로 저장되어 있고 Linked list는 위의 노드의 Next부분에 다음 노드의 위치를 저장함으로써 선형적인 데이터 자료구조를 가진다.

Python 내장 함수의 시간 복잡도에서 List의 삽입과 삭제의 시간 복잡도가 O(N)이 걸리는 것은 배열이 물리적인 데이터의 저장 위치가 연속적이어야 하므로 데이터를 옮기는 연산작업이 필요하기 때문이다.
<br>
** 참고 : 배열(Array)의 삽입과 삭제는 O(1)의 시간복잡도를 가진다. 하지만 해당 인덱스를 찾아야하는 경우 검색의 시간 복잡도인 O(N)에 해당한다.

하지만 Linked List는 데이터를 삽입, 삭제할 경우, 노드의 Next 부분에 저장한 다음 노드의 포인터만 변경해주면 되므로 배열과 비교하였을 때 Linked List가 `효율적으로` 데이터를 `삽입`, `삭제` 할 수 있다.

그러나 안타깝게도 이러한 Linked List에서 특정 위치의 데이터를 탐색하기 위해서는 첫 노드부터 탐색을 시작해야한다. 그 시간이 O(N)만큼 걸리게 되므로 탐색에 있어서는 배열이나 트리 구조에 비해 상대적으로 느리다고 할 수 있다.

<br>

### Linked List의 장점
- Linked List의 길이를 동적으로 조절 가능
- 데이터의 삽입과 삭제가 쉬움

<br>

### Linked List의 단점
- 임의의 노드에 바로 접근할 수가 없음
- 다음 노드의 위치를 저장하기 위한 추가 공간이 필요함
- Cache loclity를 활용해 근접 데이터를 사전에 캐시에 저장하기가 어려움
- Linked List를 거꾸로 탐색하기 어려움
<br>
** cache locality : `대부분 프로그램은 한번 사용한 데이터를 다시 사용할 가능성이 높고, 그 주변의 데이터도 곧 사용할 가능성이 높은 데이터 지역성`을 가지고 있다. 데이터 지역성을 활용하여 메인 메모리에 있는 데이터를 캐시 메모리에 불러와 두고, CPU가 필요한 데이터를 캐시에서 먼저 찾도록 하면 시스템 성능을 향상시킬 수 있다. 

<br>

## 단일 연결 리스트(Singly Linked List) 구현하기
단일 연결 리스트는 각 노드에 자료 공간과 한 개의 포인터 공간(다음 노드의 주소를 담는 공간)이 있고, 각 노드의 포인터는 다음 노드를 가리킨다.

### # 삽입

![Alt text](/assets/resources/linked%20list_node_insertion-001.png)

### # 삭제

![Alt text](/assets/resources/linked%20list_node_delete-001.png)


### # 코드
```python
# Node 클래스 선언
class Node (object):
    # 데이터만 인자로 전달 받고 Node 생성 시, next주소를 저장하지 않음
    def __init__(self, data):
        self.data = data
        self.next = None


# Singly Linked List 선언
class SinglyLinkedList(object):
    def __init__(self):
        self.head = None
        self.count = 0

    # 연결 리스트 끝에 새 Node 추가
    def append(self, node):
        # Node 형태가 들어오지 않았을 경우 예외 처리
        try:
            if not isinstance(node, Node):
                raise
        except TypeError as e:
            print(f"Type Error! {e}")

        # head : 생성된 연결 리스트의 제일 첫 Node
        # 첫 Node가 존재하지 않으면 입력 받은 node를 head에 추가
        if self.head == None:
            self.head = node
        else:
            current = self.head
            while current.next != None:
                current = current.next
            current.next = node

    # 출력
    def __str__(self):
        current = self.head
        string = ""

        while current:
            string += str(current.data)
            if current.next:
                string += "->"
            current = current.next
        return string

    # 원하는 데이터의 index 찾기
    def getIndex(self, data):
        current = self.head
        index = 0
        while current:
            if current.data == data:
                return index
            current = current.next
            index += 1

        return index
    
    # 정해진 index에 node추가
    def insert(self, index, node):
        current_node = self.head
        current_index = 0
        prev_node = None

        # index가 0인 경우
        if index == 0:
            # 첫 번째 node가 존재하는 경우
            if current_node:
                next_node = current_node
                self.head = node
                self.head.next = next_node
            else:
                # 첫 번째 node가 존재하지 않는 경우
                self.head = node
        else:
        # index가 0이 아닌 경우
            # 0부터 순차적으로 이전 node와 현재 node정보를 삽입할 index 번호 전까지 탐색
            while current_index < index:
                if current_node:
                    prev_node = current_node
                    current_node = current_node.next
                else:
                    break
                current_index += 1

            # 삽입할 index번호까지 current_index가 왔을 경우
            # 이전 node의 다음 주소와 삽입할 node의 다음 주소를 연결
            if current_index == index:
                node.next = current_node
                prev_node.next = node
            else:
                return -1

    # 원하는 data를 찾아 그 위치에 node 삽입
    def insertNodeAtData(self, data, node):
        index = self.getIndex(data)
        if 0 <= index:
            self.insert(index, node)
        else:
            return -1

    # 원하는 node 사제
    def delete(self, index):
        current_index = 0
        current_node = self.head
        prev_node = None
        next_node = self.head.next

        # index가 0인 경우
        if index == 0:
            self.head = next_node
        else:
            while current_index < index:
                # 현재 node의 다음이 존재하는 경우
                if current_node.next:
                    prev_node = current_node
                    current_node = next_node
                    next_node = next_node.next
                else:
                # 현재 node의 다음이 없는 경우
                    break
                current_index += 1
            if current_index == index:
                prev_node.next = next_node
            else:
                return -1

# 현재 페이지 실행
if __name__ == "__main__":
    sll = SinglyLinkedList()

    print(sll)
    sll.append(Node(1))
    sll.append(Node(2))
    sll.append(Node(3))

    print(f'sll.getIndex(1) : {sll.getIndex(1)}')
    print(f'sll.getIndex(2) : {sll.getIndex(2)}')
    print(f'sll.getIndex(3) : {sll.getIndex(3)}')
    print(sll)

    sll.insert(2, Node(5))
    print(sll)

    sll.insertNodeAtData(2, Node(4))
    print(sll)

    sll.delete(2)
    print(sll)



'''
sll.getIndex(1) : 0
sll.getIndex(2) : 1
sll.getIndex(3) : 2
1->2->3
1->2->5->3
1->4->2->5->3
1->4->5->3
'''
```

<br>

## 이중 연결리스트(Doubley Linked List) 구현하기
이중 연결 리스트는 각 노드에 `자료 공간`과 `두 개의 포인터 공간`이 있고, 각 노드의 포인터는 `이전 노드`와 `다음 노드`를 가르킨다.

![Alt text](/assets/resources/linked%20list_node-003.jpeg)

### # 이중 연결 리스트의 특징
단순 연결리스트와 이중 연결 리스트의 차이는 이전(prev)를 가리키는 포인터 공간이 추가되면서 이전 노드에 대한 정보를 알 수 있다는 점이다. 이로 인해 단순 연결 리스트로는 할 수 없는 역으로 출력하는 것이 가능하고, 특정 노드의 이전 노드를 삭제하거나 특정 노드의 이전에 삽입하는게 가능해졌다.

그러나 단순 열결 리스트 자료구조 만으로는 기능이 충분하다면 포인터 공간을 추가하여 메모리 공간을 낭비하지 않는게 좋다.

### # 삽입

![Alt text](/assets/resources/doubly%20linked%20list-001.png)


### # 삭제

![Alt text](/assets/resources/doubly%20linked%20list-002.png)

### # 코드
```python
# Node
class Node(object):
    def __init__(self, data):
        self.prev = None
        self.data = data
        self.next = None
    

# 이중 연결 리스트
class DoublyLinkedList(object):
    def __init__(self):
        self.head = None

    # 출력
    def __str__(self):
        current = self.head
        string = ""

        while current:
            string += str(current.data)
            if current.next:
                string += "->"
            current = current.next
        return string

    # node 추가
    def append(self, node):
        # 첫 번째 node가 존재하는 경우
        if self.head:
            current = self.head
            while current.next:
                current = current.next
            current.next = node
            node.prev = current
        else:
        # 첫 번째 node가 존재하지 않는 경우
            self.head = node
    
    # 원하는 index에 node 삽입
    def insert(self, index, node):
        prev_node = None
        next_node = None

        # 첫 번째 node에 추가 하는 경우
        if index == 0:
            if self.head:
                next_node = self.head
                self.head = node
                self.head.next = next_node
                next_node.prev = self.head
            else:
                self.head = node
        else:
        # 중간 or 맨 끝에 추가하는 경우
            current_index = 0
            current = self.head
            while current_index < index:
                if current:
                    prev_node = current
                    current = current.next
                else:
                    break
                current_index += 1

            if current_index == index:
                prev_node.next = node
                node.prev = prev_node
                node.next = current

                # 중간에 node를 추가하는 경우 이전 노드 정보에 삽입하는 node 추가 
                if current:
                    current.prev = node
            else:
                return -1

    # 원하는 data의 index값 얻기
    def getIndex(self, data):
        current = self.head
        current_index = 0

        while current:
            if current.data == data:
                return current_index
            current = current.next
            current_index += 1
        return current_index

    # 삽입을 원하는 데이터 위치에 넘겨준 데이터 넣기
    def insertNodeAtData(self, data, node):
        index = self.getIndex(data)
        if index >= 0:
            self.insert(index, node)
        else:
            return -1
        
    def delete(self, index):
        next_node = None
        prev_node = None
        current_index = 0

        # 삭제를 원하는 index가 1인 경우
        if index == 0:
            if self.head:
                self.head = self.head.next
                self.head.prev = None
            else:
                return -1
        current = self.head    

        while current_index < index:
            if current.next:
                prev_node = current
        

if __name__ == '__main__':
    dll = DoublyLinkedList()
    dll.append(Node(1))
    dll.append(Node(2))
    dll.append(Node(3))
    print(dll)

    dll.insert(1, Node(6))
    print(dll)
    print(dll.getIndex(6))
    print(dll.getIndex(3))

    dll.insertNodeAtData(6, Node(4))
    print(dll)

'''
1->2->3
1->6->2->3
1
3
1->4->6->2->3
'''
```

<br>

## 환형 연결 리스트(Circular Linked List) 구현하기
환형 연결 리스트라고 부르는 이 연결 리스트는 `머리와 꼬리가 연결`되어 `순환 구조`를 지닌다. 환형 연결 리스트는 Node에 포인터 공간이 두 개 일수도 있고, 한 개 일 수도 있으며 포인터 공간의 개수가 중요한 것이 아니라 노드의 Next가 None인 경우가 끊임 없이 이어진다는 의미이다. 따라서 노드가 한 개인 경우에도 무한대로 순환할 수 있다.

### # 환형 연결 리스트의 특징
복잡한 양방향 연결 리스트를 만들지 않고 간단하게 역방향 탐색을 할 수 있다는 것이 원형 연결 리스트의 장점이다. 하지만 원형 연결 리스트에서는 노드의 끝을 지나 계속 탐색하면 결국 역방향에 있는 노드로 이동할 수 있다. 다만 이렇게 수정할 경우 해당 환형 연결 리스트의 모든 요소를 확인하려고 할 경우 무한 루프에 빠질 수 있다는 단점이 존재한다.
(** 함수의 조건문을 head에 다다르기 전에 멈추도록 수정해야 한다.)

### # 삽입

![Alt text](/assets/resources/circular%20linked%20list-001.png)

### # 삭제

![Alt text](/assets/resources/circular%20linked%20list-002.png)

### # 코드
```python
# 노드
class Node(object):
    def __init__(self, data):
        self.data = data
        self.next = None

# 환형 연결 리스트
class CircularLinkedList(object):
    def __init__(self):
        self.head = None

    # 출력
    def __str__(self):
        current = self.head
        string = ""

        while current:
            string += str(current.data)
            if current.next:
                string += '->'
            if current.next == self.head:
                string = '->' + string
                break
            current = current.next
        return string

    # 새 노드 추가
    def append(self, node):
        # 첫 번째 node가 존재하는 경우
        if self.head:
            current = self.head

            # 현재의 노드가 다음이 아닌경우(노드가 1개가 아닌 경우)
            while current.next != self.head:
                current = current.next
            current.next = node
            node.next = self.head
        else:
        # 첫 번째 node가 처음 연결 리스트로 들어가는 경우 next가 자기 자신을 바라본다.
            self.head = node
            node.next = self.head
    
    # 원하는 data의 index
    def getIndex(self, data):
        index = 0
        current = self.head

        if current.data == data:
            return index
        
        index += 1
        current = current.next
        while current != self.head:
            if current.data == data:
                return index
            current = current.next
            index +=1
        return index
    
    # 원하는 위치에 node 추가
    def insert(self, index, node):
        # 연결 리스트의 첫 번째 삽입하는 경우
        if index == 0:
            if self.head:
                current = self.head.next
                current_index = 1
                while current.next != self.head:
                    current = current.next
                    current_index += 1

                node.next = self.head
                self.head = node
                current.next = self.head
                return
            else:
                self.head = node
                self.head.next = self.head
                return
        
        current_index = 1
        prev_node = self.head
        current = self.head.next

        # -> 1 -> 2 ->
        while current.next != self.head:
            if current_index == index:
                prev_node.next = node
                node.next = current
                return
            prev_node = current
            current = current.next
            current_index += 1

        current_index += 1

        if current_index == index:
            prev_node = current
            current = current.next
            prev_node.next = node
            node.next = current
            return
        return -1

    # 특정 data앞에 원하는 node 추가
    def insertNodeAtData(self, data, node):
        index = self.getIndex(data)
        if index >= 0:
            self.insert(index, node)
        else:
            return -1

    # 특정 데이터 삭제
    def removeData(self, data):
        prev_node = self.head
        current = self.head.next

        # head가 삭제해야 할 데이터의 경우
        if self.head.data == data:
            # head -1 위치 까지 탐색
            while current != self.head:
                prev_node = current
                current = current.next
            
            # head -1 위치의 next에 head의 다음 node정보
            # head 위치에 next 정보
            prev_node.next = current.next
            self.head = current.next
            return
        
        # head -1 위치 까지 탐색
        while current != self.head:
            if current.data == data:
                prev_node.next = current.next
                return
            prev_node = current
            current = current.next

        return -1

    # 특정 index의 node 제거
    def removePosition(self, index):
        if index == 0:
            if self.head:
                if self.head.next:
                    current = self.head.next
                    while current.next != self.head:
                        current = current.next
                    current.next = self.head.next
                    self.head = self.head.next
                    return
                else:
                    self.head = None
                    return
        else:
            prev_node = self.head
            current = self.head.next
            current_index = 1
            while current != self.head:
                if current_index == index:
                    next_node = current.next
                    prev_node.next = next_node
                    return
                prev_node = current
                current = current.next
                current_index += 1
            return -1


if __name__ == '__main__':
    cll = CircularLinkedList()
    cll.append(Node(1))
    print(cll)
    cll.append(Node(2))
    print(cll)

    print(cll.getIndex(1))
    print(cll.getIndex(2))

    cll.insert(2, Node(3))
    print(cll)

    cll.insertNodeAtData(2, Node(4))
    print(cll)

    cll.removeData(2)
    print(cll)

    cll.removePosition(2)
    print(cll)

'''
->1->
->1->2->
0
1
->1->2->3->
->1->4->2->3->
->1->4->3->
->1->4->
'''
```
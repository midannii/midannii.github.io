---
layout: post
title:  "[Python] 자료구조 구현[1] : stack, queue, linked list "
date:   2020-07-13
desc: " "
keywords: "python, stack, queue, linked list, tree, graph"
categories: [Python]
tags: [python, algorithm, data structure, stack, queue, linked list, tree, graph]
icon: icon-html
---

지난 학기에 자료구조 수업 때에는 c++로 자료구조를 배웠는데

python으로 구현하기로 했다! 사실 종강 직후에 하기로 한건데

눈 감았다가 뜨니 7월 중순이네요 흐흐,,,,




<br>

## 0. Node

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

이후의 자료구조의 structure가 될거야

<br>

## 1. stack

```python
class Stack:
    def __init__(self):
        self.head = None

    def is_empty(self):
        if not self.head:
            return True

        return False

    def push(self, data):
        new_node = Node(data)

        new_node.next = self.head
        self.head = new_node

    def pop(self): # LIFO
        if self.is_empty():
            return None

        ret_data = self.head.data

        self.head = self.head.next

        return ret_data

    def top(self):
        if self.is_empty():
            return None

        return self.head.data

if __name__ == "__main__":
    s = Stack()
```

stack은 LIFO 즉 last in & first out


<br>

## 2. queue

```python
class Queue:
    def __init__(self):
        self.head = None
        self.tail = None

    def is_empty(self):
        if not self.tail:
            return True

        return False

    def inqueue(self, data):
        new_node = Node(data)
        if not self.tail: # 빈 queue에 inqueue하는 경우
            self.head = new_node

        new_node.next = self.tail
        self.tail = new_node

    def dequeue(self):
        if self.is_empty(): # 빈 queue에 dequeue 하는 경우
            return None

        ret_data = self.tail.data

        self.tail = self.tail.next

        return ret_data

    def top(self):
        if self.is_empty():
            return None

        return self.tail.data

if __name__ == "__main__":
    q = Queue()
```

queue는 FIFO 즉 first in & first out

inqueue랑 dequeue만 신경써주면 stack과 거의 비슷하당



<br>


## 3. linked list

```python
# unsorted version
class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def is_empty(self):
        if not self.tail:
            return True

        return False

    def find(self, data):
        temp_node = self.head
        while (temp_node != self.tail.next):
            if temp_node.data == data:
                #print("found it: "+ str(data))
                return temp_node # 찾고자 하는 item이 있는 경우
            else:
                temp_node = temp_node.next
        return False # 찾고자 하는 item이 없는 경우

    def insert(self, data):
        new_node = Node(data)
        if not self.tail: # 빈 list에 insert하는 경우
            self.head = new_node
        else:
            self.tail.next = new_node    
        new_node.data = data
        self.tail = new_node


    def delete(self, data):
        if self.is_empty(): # 빈 list인 경우
            return None
        f = self.find(data)
        if f != False: # 삭제하려는 item이 있는 경우
            #print("let's delets" + str(f.data))
            ret_data = f.data
            if f == self.tail:# item이 맨 마지막에 존재하는 경우
                print('item is located at last')
                temp = self.head
                while(temp!= self.tail):
                    temp.data = (temp.next).data
                    temp = temp.next
                self.tail = temp
            else:
                while(f != self.tail.next):
                    f = f.next
                f = self.tail
            return ret_data
        else:
            return None

    def top(self):
        if self.is_empty():
            return None

        return self.tail.data

if __name__ == "__main__":
    q = LinkedList()
```


```python
# sorted version
class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def is_empty(self):
        if not self.tail:
            return True

        return False

    def find(self, data):
        temp_node = self.head
        while (temp_node != self.tail.next):
            if temp_node.data == data:
                #print("found it: "+ str(data))
                return temp_node # 찾고자 하는 item이 있는 경우
            else:
                temp_node = temp_node.next
        return False # 찾고자 하는 item이 없는 경우

    def insert(self, data):
        new_node = Node(data)
        if not self.tail: # 빈 list에 insert하는 경우
            self.head = new_node
            new_node.data = data
            self.tail = new_node
        else:
            temp = self.head
            while (self.tail.next != temp.next):
                if data >= (temp.next).data: # insert 위치 발견
                    temp.next = new_node
                    new_node.next = temp.next
                else:
                    temp = temp.next

    def delete(self, data):
        if self.is_empty(): # 빈 list인 경우
            return None
        f = self.find(data)
        if f != False: # 삭제하려는 item이 있는 경우
            #print("let's delets" + str(f.data))
            ret_data = f.data
            if f == self.tail:# item이 맨 마지막에 존재하는 경우
                print('item is located at last')
                temp = self.head
                while(temp!= self.tail):
                    temp.data = (temp.next).data
                    temp = temp.next
                self.tail = temp
            else:
                while(f != self.tail.next):
                    f = f.next
                f = self.tail
            return ret_data
        else:
            return None

    def top(self):
        if self.is_empty():
            return None

        return self.tail.data

if __name__ == "__main__":
    q = LinkedList()
```


linkes list는 일반 list에서 조금 더 발전해서, node를 이용한 list당

sorted 랑 unsorted 두가지 버전으로 만들어봤당

약간 응용해서 doubly linked list도 가능하고 insert와 delete를 수정해서 sort도 가능하당

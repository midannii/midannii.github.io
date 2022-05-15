---
layout: post
title:  "[Python] 자료구조 구현[2] : tree, heap, graph "
date:   2020-07-14
categories: algorithm
---


[저번](https://midannii.github.io/python/2020/07/13/datastructure.html)에 이어서 이번에는 tree, graph, heap을 구현해보자



이론을 간단하게 정리해 본 그림이다.


![그림](static/assets/img/blog/python/ds.jpeg)


<br>

## 4. Tree


tree를 구현하는 방법은 크게 두가지가 있다.

doubly node에 left, data, node를 할당하는것과

list를 만들어서 list에다가 정보를 저장하는 방법 !




```python
class Tree:
    def __init__(self, root):
        self.root = root
        self.node = 0

    def isEmpty(self):
        if not self.root:
            return True
        return False

    def CountNode(self):
        if isEmpty(self):
            return 0
        else:
            CountNode(self.left) + CountNode(self.right)+1

    def FindItem(self, item):
        if isEmpty(self):
            return false
        elif (item < (self.root).data):
            FindItem(self.left)
        elif (item > (self.root).data):
            FindItem(self.right)
        else:
            return true

    def InsertItem(self, item):
        if isEmpty(self):
            self.root = TreeNode(item)
        else:
            if (item<(self.root).data):
                InsertItem((self.root).left, item)
            else:
                InsertItem((self.root).right, item)

    def DeleteItem(self, item):
        temp = self.root
        if not isEmpty(self):
            if not temp.left:
                temp = temp.right
            elif not temp.right:
                temp = temp.left
            else:
                while(temp.data < item):
                    temp = temp.right
                temp.data = item
                Delete(temp.left, item)

def Delete(tree, item):
    if item<tree.data:
        Delete(tree.left, item)
    elif item>tree.data:
        Delete(tree.right, item)
    else:
        tree.DeleteItem(item)
```





<br>

## 5. Graph


graph는 matrix나 linked list로 구현한당

matrix는 각각 item을 행, 열로 둔 후 사이 edge가 있으면 그 칸에 weight을 채워넣으면 되고

linked list는

사실 다 장단점이 있지 ! node가 많으면 matrix가, 적으면 linked list가 효율 면에서 유리하당



```python
# list version
class Graph:
    def __init__(self):
        self.items = []
        self.nums = 0
        self.mat = []

    def isEmpty(self):
        if not self.root:
            return True
        return False

    def AddVertex(self, item):
        (self.items).append(item)
        self.nums +=1
        temp = [0]*self.nums
        self.mat.append(temp)


    def DeleteVertex(self, item):
        if item in self.items:
            a = self.items.index(item)
            while(a!=self.nums):
                self.items[a] = self.items[a+1]
                self.mat[a] = self.mat[a+1]
                a +=1
            b = self.items.index(item)
            self.nums-=1
            if b!=0:
                for i in range(self.nums):
                    matrix[i] = matrix[i][:b-1]+matrix[b:]
            else:
                for i in range(self.nums-1):
                    matrix[i] = matrix[i+1]
            matrix[self.nums] = [0]*self.nums

    def AddEdge(self, v1, v2, e):
        if v1 in self.items and v2 in self.items:
            a = self.items.index(v1)
            b = self.items.index(v2)
            self.mat[a][b] = e

    def DeleteEdge(self, v1, v2):
        if v1 in self.items and v2 in self.items:
            self.mat[a][b] = 0

    def WeightIs(self, v1, v2):
        if v1 in self.items and v2 in self.items:
            return self.mat[a][b]

```



<br>

## 6. heap

Heap 은 tree와 비슷한데,

root가 최댓값인 min heap이냐, 최대값인 max heap이냐에 따라 다른듯

이건 insert랑 delete를 손봐주면 될 거같다!

[여기](# https://geonlee.tistory.com/72)를 참고했당



```python
# https://geonlee.tistory.com/72
class Heap:
    def __init__(self, root):
        self.root = root

    def isEmpty(self):
        if not self.root:
            return True
        return False

    def FindItem(self, item):
        if self.root is None or root.data == key:
            return root is not None
        elif key < root.data:
            return self.Find_value(root.left, key)
        else:
            return self.Find_value(root.right, key)


    def InsertItem(self, item):
        self.root = self.insert_value(self.root, data)

    def insert_value(self, node, data):
        if node is None:
            node = Node(data)
        else:
            if data <= node.data:
                node.left = self.insert_value(node.left, data)
            else:
                node.right = self.insert_value(node.right, data)
        return node


    def DeleteItem(self, item):
        self.root, deleted = self.delete_value(self.root, key)

    def delete_value(self, node, key):
        if node is None:
            return node, False

        deleted = False
        if key == node.data:
            deleted = True
            if node.left and node.right:
                # replace the node to the leftmost of node.right
                parent, child = node, node.right
                while child.left is not None:
                    parent, child = child, child.left
                child.left = node.left
                if parent != node:
                    parent.left = child.right
                    child.right = node.right
                node = child
            elif node.left or node.right:
                node = node.left or node.right
            else:
                node = None
        elif key < node.data:
            node.left, deleted = self.delete_value(node.left, key)
        else:
            node.right, deleted = self.delete_value(node.right, key)
        return node, deleted


```



<br>

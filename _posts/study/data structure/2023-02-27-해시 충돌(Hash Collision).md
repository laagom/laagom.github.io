---
layout: post
title:  "[Data Structure] 해시 충돌(Hash Collision)"
subtitle:   "해시 충돌(Hash Collision)"
date: 2023-02-27
categories: study
tags: data_structure
comments: true
image: /assets/img/lagom.png
related_posts:
    - /_posts/study/computer_science/2023-01-31-중앙처리장치(CPU) 작동 원리.md
    - /_posts/study/computer_science/2023-01-31-중앙처리장치(CPU) 작동 원리.md
---

# 해시 충돌(Hash Collision)
앞서 [해시(Hash)가 무엇인지?](https://laagom.github.io/study/2023/02/24/%ED%95%B4%EC%8B%9C(Hash).html) 어떻게 사용하는지 기본적인 자료구조 해시(Hash)에 대해 알아 보았다.

Hash Table은 몇가지 기본 기능을 제공하는데
- 데이터 삽입(저장)<br>
    Hash Table에서 데이터를 저장하기 위해 먼저 Hash Function을 이용하여 key값을 hash로 변경한다. 이후 미리 준비해둔 저장소(bucket, slot) 중에 알맞는 hash값을 찾아 value를 저장한다. 이 과정은 `시간복잡도 O(1)` 을 가진다.
- 데이터 삭제<br>
    저장되어 있는 값을 삭제하기 위해서 bucket에서 삭제하려고 하는데 key와 매칭되는 value값을 찾아서 삭제한다. 이러한 과정은 `시간복잡도 O(1)`을 가진다.
- 데이터 검색<br>
    key를 이용하여 value를 찾아내는 과정이며, 먼저 key값과 hash function을 이용해 hash를 찾아내고 해당 hash로 value값을 찾을 수 있다. 이 과정은 `시간복잡도 O(1)`을 가진다.

위와 같이 hash table은 데이터 저장, 삭제, 검색이 많이 필요한 경우 자주 사용되며 각각 기능마다 `시간복잡도 O(1)`을 가지는 아주 효율적인 자료구조이다.

하지만 효율적인 만큼 `단점도` 존재하는데 먼저 hash값들을 저장할 공간(bucket)을 지정해야하므로 일반적으로 `저장 공간이 많이 필요`로한다.

또한 여러 key에 해당하는 hash(주소)가 동일한 경우 `해시 충돌(Hash Collision)`이 발생한다.

## 해시 충돌(Hash Collision)이란?
위에서 hash table의 단점으로 언급한 여러 key에 해당하는 hash가 동일한 경우를 `해시 충돌(Hash Collision)`이라고 부른다.

![Alt text](/assets/resources/hash_collision-001.png)

[비둘기집의 원리](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=alwaysneoi&logNo=100164276686)로 인해 hash를 이용한 자료구조에서는 무조건적으로 일어날 수 밖에 없는 현상이라고 한다. 

해시 함수(hash function)가 모든 입력에 대해 항상 다른 해시 값을 부여할 수 있다면 이상적이지만 모든 데이터에 대해 알고 있지 않다면 완벽한 해시 함수(hash function)를 작성하는 것은 불가능하다.

위의 이미지와 같이 John Smith와 Sandra Dee가 해시 함수(hash function)을 통과하면 02라는 동일한 hash값을 부여받게 된다. 즉, 데이터는 많은데 공간이 제약되어 있어 동일한 hash값을 부여 받아 충돌이 일어나는 것이다. 공간을 넓힐 수 있다면 최대한 넓히는 게 좋지만 현실의 메모리는 한정되어 있기때문에 알고리즘을 설계할때 충돌을 최소화할 수 있는지 충돌이 발생했을 때 어떤 방식으로 처리할지를 잘 생각해 놔야 한다.

<br>

## 해시 테이블(Hash Table) 구현
먼저 Python을 이용하여 Hash 데이터 구조와 여러 기능을 간단히 구현해보자. 아래는 Python을 이용하여 hash table을 List로 구현한 것이다.
```python
class HashTable:
    def __init__(self, table_size):
        self.size = table_size      # 총 공간(bucket) 수
        self.hash_table = [0 for a in range(self.size)]     # 빈 hash table 생성     
    
    def getKey(self, data):     # key(문자) 첫 문자의 유니코드를 반환
        self.key = ord(data[0])
        return self.key
    
    def hashFunction(self, key):    # key의 hash 값을 반환
        return key % self.size
    
    def getAddress(self, key):      # hash address를 반환
        myKey = self.getKey(key)
        hash_address = self.hashFunction(myKey)
        return hash_address
    
    def save(self, key, value):     # 데이터 저장
        hash_address = self.getAddress(key)
        self.hash_table[hash_address] = value

    def read(self, key):    # key에 해당하는 value 반환
        hash_address = self.getAddress(key)
        return self.hash_table[hash_address]
    
    def delete(self, key):
        hash_address = self.getAddress(key)

        if self.hash_table[hash_address] != 0:
            self.hash_table[hash_address] = 0
            return True
        else:
            return False

if __name__ == '__main__':
    h_table = HashTable(8)
    h_table.save('a', 1111)
    h_table.save('b', 2222)
    h_table.save('c', 3333)
    h_table.save('d', 4444)

    print(h_table.hash_table) # [0, 1111, 2222, 3333, 4444, 0, 0, 0]

    print(h_table.read('d')) # 4444

    h_table.delete('d')

    print(h_table.hash_table) # [0, 1111, 2222, 3333, 0, 0, 0, 0]
```

<br>

## 해시 충돌 시 알고리즘 2가지
충돌은 불가피하니 충돌 시 데이터 처리하는 2가지 방법이 존재한다.

<br>

### 🔖 Chaning(Open Hashing)
해시 테이블 저장공간 외의 공간을 활용하는 기법인 Chaning(체이닝)방법이다. 

즉, 충돌이 일어나면, Linked List로 데이터를 추가로 뒤에 연결시켜서 저장하는 기법이다. 
![Alt text](/assets/resources/hash%20collision(linked%20list)-001.png)

Python은 List가 있기 때문에 Linked List로 반드시 만들 필요는 없다. 모든 공간을 List형식으로 만들어서 값이 중복되면 리스트에 하나씩 추가하는 방식으로 구현이 가능하다.

`장점`은 모든 값을 넣을 수 있다는 것과 데이터가 일단 저장이 되므로 메모리 소모가 적다.

`단점`은 Chaing을 통해 hash table을 생성하였을 때 기능들의 시간 복잡도가 O(n)까지 늘어날 수 있다.

하지만 충돌이 일어나는 곳에 계속 데이터가 늘어나게 되면 한 공간에만 저장되는 쏠림 현상이 일어날 수 있어서 검색을 할 때는 좋지 않은 현상이다.

<br>

### 🔖 Chaning 과정(동작 방식)
- 키의 해시값을 계산한다.
- 해시 값에 해당하는 리스트 인덱스를 구한다.
- 같은 해시 값을 가진 키가 있다면(Hash Collision) List로 연결한다.

```python
class Chaining:
    def __init__(self, table_size):
        self.size = table_size
        self.hash_table = [0 for a in range(self.size)]

    def getKey(self, data):
        self.key = ord(data[0])
        return self.key
    
    def hashFunction(self, key):
        return key % self.size
    
    def getAddress(self, key):
        myKey = self.getKey(key)
        hash_address = self.hashFunction(myKey)
        return hash_address
    
    def save(self, key, value):
        hash_address = self.getAddress(key)

        if self.hash_table[hash_address] != 0: # 해시 충돌 시
            for a in range(len(self.hash_table[hash_address])): # 빈 bucket 탐색
                if self.hash_table[hash_address][a][0] == key:
                    self.hash_table[hash_address][a][1] = value
                    return
            self.hash_table[hash_address].append([key, value])
        else:
            self.hash_table[hash_address] = [[key, value]]

    def read(self, key):
        hash_address = self.getAddress(key)

        if self.hash_table[hash_address] != 0: # 해시 충돌 시
            for a in range(len(self.hash_table[hash_address])):
                if self.hash_table[hash_address][a][0] == key: # 일치하는 key 값 찾음 
                    return self.hash_table[hash_address][a][1] # 일치하는 value 반환
            return False
        else:
            return False
        
    def delete(self, key):
        hash_address = self.getAddress(key)

        if self.hash_table[hash_address] != 0:
            for a in range(len(self.hash_table[hash_address])):
                if self.hash_table[hash_address][a][0] == key:
                    if len(self.hash_table[hash_address]) == 1:
                        self.hash_table[hash_address] = 0
                    else:
                        del self.hash_table[hash_address][a]
                    return
            return False
        else:
            return False
        
if __name__ == '__main__':
    h_table = Chaining(8)

    h_table.save('aa', '1111')
    print(h_table.read('aa')) # 1111

    data1 = 'aa'
    data2 = 'ab'

    print(ord(data1[0]))   # 97
    print(ord(data2[0]))   # 97  같은 hash 값을 가짐 -> 해시 충돌

    h_table.save('ab', '2222')
    print(h_table.hash_table)  # [0, [['aa', '1111'], ['ab', '2222']], 0, 0, 0, 0, 0, 0]

    print(h_table.read('aa'))  # 1111
    print(h_table.read('ab'))  # 2222

    h_table.delete('aa')
    print(h_table.hash_table)  # [0, [['ab', '2222']], 0, 0, 0, 0, 0, 0]

    print(h_table.delete('Data')) # False
```

실제로 8개의 공간에 9개의 데이터를 넣어보면 무조건 1개 이상 충돌이 날수밖에 없기 때문에 chaning기법에 의해 공간안에 리스트 길이가 2이상인 경우가 발생한다.

<br>

### 🔖 Linear probing 기법
Linear probing은 -폐쇄 해시 또는 Close Hashing 기법 중 하나로 Hash Table 저장공간 안에서 충돌 문제를 해결하는 기법이다. 즉, 충돌이 일어나면 해당 Hash Address의 다음 address부터 맨 처음 나오는 빈 공간에 저장하는 기법이다. chaining처럼 한쪽 쏠림현상을 없애고 저장공간 활용도를 높이기 위한 기법이다.

![Alt text](/assets/resources/linear-probing.png)
```python
class OpenAddressing:
    def __init__(self, table_size):
        self.size = table_size
        self.hash_table = [0 for a in range(self.size)]
        
    def getKey(self, data):
        self.key = ord(data[0])
        return self.key
    
    def hashFunction(self, key):
        return key % self.size

    def getAddress(self, key):
        myKey = self.getKey(key)
        hash_address = self.hashFunction(myKey)
        return hash_address
    
    def save(self, key, value):
        hash_address = self.getAddress(key)
        
        if self.hash_table[hash_address] != 0:    # hash 의 값이 이미 존재할때 (해시충돌)
            for a in range(hash_address, len(self.hash_table)):
                if self.hash_table[a] == 0:
                    self.hash_table[a] = [key, value]
                    return
                elif self.hash_table[a][0] == key:
                    self.hash_table[a] = [key, value]
                    return
            return None
        else:    # hash 가 비었을때
            self.hash_table[hash_address] = [key, value]
            
    def read(self, key):
        hash_address = self.getAddress(key)
        
        for a in range(hash_address, len(self.hash_table)):
            if self.hash_table[a][0] == key:
                return self.hash_table[a][1]
        return None
    
    def delete(self, key):
        hash_address = self.getAddress(key)
        
        for a in range(hash_address, len(self.hash_table)):
            if self.hash_table[a] == 0:
                continue
            if self.hash_table[a][0] == key:
                self.hash_table[a] = 0
                return
        return False


if __name__=='__main__':
    
    h_table = OpenAddressing(8)

    data1 = 'aa'
    data2 = 'ab'
    print(ord(data1[0]), ord(data2[0]))    ## 97 97  -> 해시 충돌

    h_table.save('aa', '1111') 
    h_table.save('ab', '2222')

    print(h_table.hash_table)  # [0, ['aa', '1111'], ['ab', '2222'], 0, 0, 0, 0, 0]

    print(h_table.read('ab'))  # 2222

    h_table.delete('aa') 
    print(h_table.hash_table)  # [0, 0, ['ab', '2222'], 0, 0, 0, 0, 0]
```
위의 chaining와 다르게 한 저장소에 저장되는 것이 아니라 다음 저장소에 되는 모습을 볼 수 있고 삭제 시에도 동일한 hash 값을 받아 저장되었더라도 delete()처리 시 해당 key와 동일한 값을 찾아 삭제할 수 있게 하였다. 

<br>

## 충돌예방을 위해 할 수 있는 것
충돌 방지를 위해서는 2가지 조치를 취할 수 있다.
- 공간을 늘린다.
- 해시함수를 좋은 걸로 사용한다.

공간을 최대한 늘리던지 그게 아니면 충돌이 잘 안되는 해시함수를 사용하면 된다.

<br>

## 해시함수(Hash Function)의 중요성
해시테이블(Hash Table)의 충돌은 불가피하다. 충돌은 왠만하면 피하는게 상책이다. 그러기 위해선 해시함수를 잘 정해서 충돌을 방지해야한다.

해시함수를 거쳐 나온 hash_address를 고정 값으로 만드는 것도 한 방법이다. hash()함수는 키 값 고정이 안되서 이를 고정값으로 만들 수 있는 라이브러리가 있다. hashlib라는 라이브러리인데 SHA1, SHA224, SHA256, SHA384, SHA512 등 좋은 해시 함수를 사용할 수 있다.

<br>

## 키값을 고정시켜야 하는 이유(시간 복잡도)
키 값을 고정시켜야 하는 이유는 충돌 기복을 줄일 수 있기 때문이다.
- 충돌이 하나도 일어나지 않았을 경우 O(1)
- 충돌이 모두 일어나면 O(n)

데이터가 많으면 많을수록 충돌이 일어날 확률이 높아지는데 충돌이 안일어난다면 시간복잡도를 많이 줄일 수 있다.

<br>

## 마치며
해시테이블(Hash Table)의 충돌 방지 방법에 대해 알아보았다. 위에서 알아본 방법들은 자료구조와 알고리즘을 위해 학습한거지만 Python에서는 Dictionary로 하기 때문에 아마 거의 사용하지 않을 것 같다.
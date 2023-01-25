---
layout: post
title:  "[Algorithm] 거품 정렬 (Bubble Sort)"
subtitle:   "거품 정렬 (Bubble Sort)"
date: 2023-01-25
categories: study
tags: algorithm
comments: true
related_posts:
image: /assets/img/lagom.png
---

# 거품 정렬 (Bubble Sort)

## <span style='color:hsl(350, 100%, 66%);'>개념</span>
거품 정렬은 뒤에서 부터 앞으로 정렬을 해나가는 구조를 가지고 있다. (오름차순)즉, 배열 내의 인접한 값들을 앞뒤로 비교하며 자리를 바꾸는데 맨 뒷자리로 제일 큰 값을 보내는 작업을 수행한다. 이렇게 하나씩 큰 값을 뒤쪽으로 보내 채워 나가며 앞으로 정렬해 왔을 때 작은 수 부터 큰 수 까지 오름차순으로 정렬된 배열을 가지는 정렬이다. 큰 값을 계속해서 뒤로 보내는 모습이 마치 방울이 이동하는 것과 같이 보여서 거품 정렬이라는 이름이 붙어졌다.

이제 하나의 배열을 가지고 위에서 설명한 거품정렬이 어떻게 진행되는지 알아보자. 다음과 같이 `1`부터 `5`까지 총 5개의 숫자가 들어있는 배열이 존재한다.
```text
[3, 2, 4, 5, 1]
```
먼저 3, 2를 비교, 3이 2보다 크기 때문에 자리를 바꾸며 큰 값을 뒤로 보내준다.
```text
[3, 2, 4, 5, 1]

3 > 2 => switch 
[2, 3, 4, 5, 1]
```
그 다음 3, 4를 비교, 3은 4보다 크지 않기 때문에 그대로 유지한다.
```text
[2, 3, 4, 5, 1]

3, 4 => keep     
```
그 다음 4, 5를 비교, 4는 5보다 크지 않기 때문에 그대로 유지한다.
```text
[2, 3, 4, 5, 1]

4, 5 => keep
```
그 다음 5, 1를 비교, 5는 1보다 크기 때문에 자리를 바꾸며 큰 값을 뒤로 보내준다.
```text
[2, 3, 4, 5, 1]

5 > 1 => switch
[2, 3, 4, 1, 5]
```

이렇게 값을 처음부터 비교해가며 앞의 값이 뒤의 값보다 큰 경우 자리를 바꿔주면 제일 큰 값을 맨 뒤로 보내줄 수 있다. 위의 과정은 제일 큰 값을 맨 뒤로 보내주는 작업이며 그다음은 두번째 큰 값을 제일 큰 값 앞으로 보내주는 과정을 진행해 주면 된다.

```text
origin: [3, 2, 4, 5, 1]
-----------------------
loop 1: [2, 3, 4, 1, 5]
                   *
loop 2: [2, 3, 1, 4, 5]
                *  *
loop 3: [2, 1, 3, 4, 5]
             *  *  *
loop 4: [1, 2, 3, 4, 5]
          *  *  *  *
loop 5: [1, 2, 3, 4, 5]
       *  *  *  *  *
```

<br>

## <span style='color:hsl(350, 100%, 66%);'>GIF로 이해하는 Bubble Sort</span>

<img src="/assets/resources/bubble-sort-001.gif">

<br>

## <span style='color:hsl(350, 100%, 66%);'>특징</span>
1. 거품 정렬은 점점 큰 값들이 뒤에서 앞으로 쌓여 이동하기 때문에 정렬의 범위가 하나씩 줄어든다. 다음 loop에서 제일 뒤로 간 큰 값은 비교를 할 필요가 없기 때문이다.
2. 제일 작은 값을 맨 앞으로 이동시키는 선택 정렬과 정렬 방향이 반대이다.
3. 선택정렬에 비해 자리 변경이 더 많이 일어난다.
4. 위의 loop 4, loop 5를 보면 필요없는 과정이 있어 최적화가 필요하다.

<br>

## <span style='color:hsl(350, 100%, 66%);'>복잡도</span>
1. 거품 정렬은 별도의 추가 공간을 사용하지 않고 배열안에서 값들의 위치가 변경되기 때문에 `0(1)`의 공간 복잡도를 가진다.
2. 시간 복잡도는 반복문을 통해 맨 뒤에서 맨 앞까지 모든 인덱스에 접근하기 때문에 기본적으로 `0(N)` 시간을 소모하며, 하나의 루프에서는 인접한 값들의 대소 비교 및 자리 교대를 위해서 `0(N)` 시간을 필요하게 된다. 따라서 거품 정렬은 총 `0(N^2)`의 시간 복잡도를 가진다.
3. 정렬이 이미 완료된 배열인 경우에는 0(N) 시간 복잡도를 가진다.

<br>

## <span style='color:hsl(350, 100%, 66%);'>Python 코드</span>
선택 정렬과 마찬가지로 두 개의 반복문이 필요하다. 내부 반복문에서는 첫번째 값부터 이전 패스에서 뒤로 보내놓은 값이 있는 위치 전까지 앞뒤 값을 계속해서 비교해나가면서 앞의 값이 뒤의 값보다 클 경우 자리 변경(switch)을 한다. 외부 반복문에서는 뒤에서 부터 앞으로 정렬 범위를 `n-1`부터 `1` 까지 줄여나간다.
```python
def bubbleSort(array):
    length = len(array)-1

    for i in range(length):
        for j in range(length-i):
            if array[j] > array[j+1]:
                array[j],array[j+1] = array[j+1], array[j]

    return array

bubble_arry = [10, 1, 9, 2, 8, 3, 7, 4, 6, 5]

print(bubbleSort(bubble_arry))
```

<br>

## <span style='color:hsl(350, 100%, 66%);'>최적화</span>
이전 loop에서 앞뒤 자리 변경(switch)이 한 번도 일어나지 않았다면 정렬도지 않는 값이 하나도 없었다고 할 수 있다. 따라서 이러한 경우 loop를 수행하지 않아도 된다.
```text
origin: [1, 2, 3, 5, 4]
------------------------
 loop 1: [1, 2, 3, 4, 5] => switch 있음
                      *
 loop 2: [1, 2, 3, 4, 5] => switch 없음
                   *  *
=> 이전 패스에서 loop이 한 번도 없었으니 종료
```

```python
import timeit

def time_decorator(func):

    def exec_func(*args, **kwargs):
        start_time = timeit.default_timer()
        result = func(*args, **kwargs)
        print(timeit.default_timer() - start_time)
        return result
    return exec_func

@time_decorator
def bubbleSort(array):
    length = len(array)-1

    for i in range(length):
        switch = False
        for j in range(length-i):
            if array[j] > array[j+1]:
                array[j], array[j+1] = array[j+1], array[j]
                switch = True
        if not switch:
            break

    return array

bubble_arry = [10, 1, 9, 2, 8, 3, 7, 4, 6, 5]

print(bubbleSort(bubble_arry))
```

<br>

## <span style='color:hsl(350, 100%, 66%);'>최적화</span>
최적화 하여 코드를 변경하고 시간을 측정해 봤을때 실행 속도가 많이 빨라졌다는 느낌은 없었다. 하지만 최적화 전보다 빈번하게 더 시간이 단축되어 실행되는 경우가 발생한 것을 보니 코드 최적화가 조금은 된 것으로 보인다.
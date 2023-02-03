---
layout: post
title:  "[Algorithm] 선택 정렬 (Selection Sort)"
subtitle:   "선택 정렬 (Selection Sort)"
date: 2023-02-02
categories: study
tags: algorithm
comments: true
related_posts:
image: /assets/img/lagom.png
---

# 선택 정렬 (Selection Sort)
정렬 알고리즘 중에서 가장 직관적이고 쉽게 구현이 가능한 선택 정렬(Selection Sort)에 대해 알아보려고 한다.

<br>

## <span style='color:hsl(350, 100%, 66%);'>개념</span>
선택 정렬은 [거품 정렬](https://github.com/laagom/Tech-Knowledge/blob/main/Algorithm/%EA%B1%B0%ED%92%88%20%EC%A0%95%EB%A0%AC(Bubble%20Sort).md)과 많이 유사하다. 해당 순서에 원소를 넣을 위치는 이미 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘이다. `거품정렬`의 경우 앞에서 서로 인접한 두개의 요소를 비교하여 제일 큰 값부터 뒤로 보내는 알고리즘이었다면, `선택 정렬`은 앞에서 부터 제일 작은 값을 순차적으로 나열하는 방식의 알고리즘이다.

이제 하나의 배열을 가지고 위에서 설명한 `선택 정렬`이 어떻게 진행되는지 알아보자.

- 주어진 배열 중에 최소값을 찾는다.
- 그 값을 맨 앞에 위치한 값과 교체한다.
- 맨 처음 위치를 뺀 나머지 배열을 같은 방법으로 교체한다.

다음과 같이 4명의 친구들의 키를 알고 있는데 순서대로 세우고 싶다. 
```text
170cm, 180cm, 150cm, 160cm
```
위와 같이 키를 알고 있는 4명의 친구들 중 키가 제일 작은 `150cm`인 친구를 맨 앞에 세운다.
```text
150cm (4명 중 제일 작음)
```
키가 `150cm`인 친구는 맨 앞에 세웠으니, 이제 나머지 세 친구를 비교하여 키가 두 번째로 작은 `160cm`인 친구를 그 다음에 세운다.
```text
150cm, 160cm (150cm 빼고 남은 3명 중 제일 작음)
```
이제 키가 `70cm`, `180cm`인 친구만 남았다. 둘 중 `170cm`인 친구가 더 작기 때문에 그 다음에 세우고 `180cm`인 친구를 마지막에 세운다.

<br>

즉, 크기 n의 배열이 주어졌을 때, index 0부터 n-1까지의 모든 index i에 대해, i번째 부터 n-1번째까지 값 중 가장 작은 값을 구해서 index i에 놓으면 정렬된 배열을 얻을 수가 있다. `모든 index에 대해서 그 index에 위치시킬 값을 "선택"하기 때문에 이 정렬 알고리즘을 "선택 정렬" 또는 "Selection Sort"`라고 부른다.

<br>

## <span style='color:hsl(350, 100%, 66%);'>GIF로 이해하는 Selection Sort</span>


<img src="/assets/resources/selection-sort-001.gif">

<br>

## <span style='color:hsl(350, 100%, 66%);'>특징</span>
1. 선택 정렬은 정렬된 값을 배열의 맨 앞부터 하나씩 채워나가게 된다. 따라서, 뒤에 있는 `index로 갈수록 비교 범위가 하나씩 점점 줄어드는 특성`을 가지고 있다. 이 특성은 최대 값을 뒤에서 부터 채워나가는 버블 정렬과 동일한 특성을 가진다.
2. 입력 배열이 이미 정렬되어 있건 말건 관계없이 동일한 연산량을 가지고 있기 때문에 최적화 여지가 적어서 시간복잡도가 `O(N^2)`으로 비효율적이다.

<br>

## <span style='color:hsl(350, 100%, 66%);'>복잡도</span>
1. 선택 정렬은 별도의 추가 공간을 사용하지 않고 주어진 배열이 차지하고 있는 공간 내에서 값들의 위치만 바꾸기 때문에 `O(1)`의 공간 복잡도를 가진다.
2. 시간 복잡도는 우선 루프문을 통해 모든 인덱스에 접근해야 하기 때문에 기본적으로 `O(N)`을 시간을 소모하며, 하나의 루프에서는 현재 인덱스의 값과 다른 인덱스의 값들과 비교하여 최소 값을 찾은 후 현재 인덱스에 있는 값과 서로 자리를 변경해야 하기 때문에 `O(N)`의 시간이 필요하게 된다. 최종적으로 선택 정렬은 `O(N^2)`의 시간  복잡도를 가지는 정렬 알고리즘이다.
<br>

        데이터의 개수가 n개라고 했을 때,
        첫 번째 회전에서의 비교횟수 : 1 ~ (n-1) => n-1
        두 번째 회전에서의 비교횟수 : 2 ~ (n-1) => n-2
        ...

        (n-1) + (n-2) + .... + 2 + 1 => n(n-1)/2

<br>

## <span style='color:hsl(350, 100%, 66%);'>Python 코드</span>
거품 정렬과 마찬가지로 두 개의 반복문이 필요하다. 내부 반목문에서는 현재 index부터 마지막 index까지 최소값의 index를 찾아내고, 외부 반복문에서는 이 최소값의 index와 현재 index에 있는 값을 변경한다. 외부 반복문에서는 index `i`를 `0`에서 `n-2`(또는 `n-1`. 마지막 index에서는 남는 갓이 하나 밖에 없기 때문에 대소에 지장 없음)까지 진행, 내부 반복문에서 이미 정렬된 값들에서는 관심이 없기 때문에 index `j`를 `i`에서 `n-1`까지 진행시킨다. 각 index에 대해서 최소값을 찾기 위해 대소 비교는 여러번 일어나나 상호 교대는 한번만 일어난다.
```python
def selection_sort(array):
    for i in range(len(array)-1):
        min_idx = i

        for j in range(i+1, len(array)):
            if array[j] < array[min_idx]:
                min_idx = j
                
        array[i], array[min_idx] = array[min_idx], array[i]

    return array

height = [190, 170, 150, 160, 180]
print(selection_sort(height))
```


<br>

## <span style='color:hsl(350, 100%, 66%);'>결론</span>
버블 정렬과 유사하지만 조금 더 빠른 선택 정렬에 대해 정리해 보았다. 시간 측정을 해 보았지만 정말 미묘하게 더 빠른 타임을 가진다.

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
def selection_sort(array):
    for i in range(len(array)-1):
        min_idx = i

        for j in range(i+1, len(array)):
            if array[j] < array[min_idx]:
                min_idx = j
                
        array[i], array[min_idx] = array[min_idx], array[i]

    return array

select_arry = [10, 1, 9, 2, 8, 3, 7, 4, 6, 5]
print(selection_sort(select_arry))

# 6.4999330788850784e-06
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
---
layout: post
title:  "[Algorithm] 병합 정렬 (Merge Sort)"
subtitle:   "병합 정렬 (Merge Sort)"
date: 2023-03-07
categories: study
tags: algorithm
comments: true
related_posts:
image: /assets/img/lagom.png
---

# 병합 정렬(Merge Sort)
합병 정렬이라고도 부르며, [퀵 정렬(Quick Sort)](https://laagom.github.io/study/2023/03/05/%ED%80%B5-%EC%A0%95%EB%A0%AC(Quick-Sort).html)과 동일하게 분할 정복 방법을 통해 구현한다.

<br>

### <span style='color:hsl(350, 100%, 66%);'>개념</span>
퀵 정렬과 마찬가지로 빠른 정렬로 분류되며 많이 사용되고 언급되는 정렬 방식이다. 퀵 정렬과는 반대로 `안정 정렬`에 속한다. 병합 정렬은 `분할 정복(Divide and Conquer) 기법`과 `재귀 알고리즘`을 이용해서 정렬하는 알고리즘이다.

주어진 배열을 원소가 하나 밖에 남지 않을 때까지 쪼갠 후 다시 크기 순으로 재배열 하면서 원래 크기의 배열로 합친다.

이제 하나의 배열을 가지고 위에서 설명한 `병합 정렬`이 어떻게 진행되는지 알아보자.

예를 들어, 다음과 같이 `1`부터 `8`까지 총8개의 숫자가 들어있는 배열에 있다고 가정해보자.
```text
[6, 5, 3, 1, 8, 7, 2, 4]
```
위의 배열을 반토막 내며 반복해서 쪼개주자.
```text
[6, 5, 3, 1] [8, 7, 2, 4]

[6, 5] [3, 1] [8, 7] [2, 4]

[6] [5] [3] [1] [8] [7] [2] [4]
```

이렇게 반토막 내며 쪼개면 결과적으로 배열이 가진 원소 수 만큼 배열이 나눠진다. 이렇게 더 이상 쪼갤 수 없을때가 되면 두 개씩 병합을 시작할 것이다. 합칠 때는 작은 숫자가 앞에, 큰 숫자가 뒤에 위치시킨다.

```text
[5, 6] [1, 3] [7, 8] [2, 4] 
```
이제 4개의 배열을 2개로 합친다. 각 배열 내에서 가장 작은 값 2개를 비교해서 더 작은 값을 먼저 선택하면 자연스럽게 크기 순으로 선택이 된다.

```text
[1, 3, 5, 6] [2, 4, 7, 8]
```
최종적으로 2개의 배열도 마찬가지로 크기 순으로 선택해가면서 하나로 합치면 정렬된 배열을 얻을 수 있다.

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

<br>

### <span style='color:hsl(350, 100%, 66%);'>GIF로 이해하는 Merge Sort</span>

<img src="/assets/resources/merge-sort-001.gif">

<br>

### <span style='color:hsl(350, 100%, 66%);'>특징</span>
1. 알고리즘을 큰 그림에서 보면 분할(split) 단계와 병합(merge) 단계로 나눌 수 있으며, 단순히 중간 인덱스를 찾아야하는 분할 비용보다 모든 값들을 비교해야하는 병합 비용이 더 크다.
2. 예제에서 보이는 것과 같이 8->4->2->1 식으로 전반적인 반복의 수는 점점 절반으로 줄어들기 때문에 O(logN)시간이 필요하며, 각 패스에서 병합할 때 모든 값들을 비교해야 하므로 O(N) 시간이 소모된다. 따라서 총 시간 복잡도는 O(NlogN)이다.
3. 두 개의 배열을 병합 할 때 병합 결과를 담아 놓을 배열이 추가로 필요하다. 따라서 공간 복잡도는 O(N)이다.
4. 다른 정렬 알고리즘과 달리 인접한 값들 간에 상호 자리 교대가 일어나지 않는다.

<br>

### <span style='color:hsl(350, 100%, 66%);'>Python 코드</span>
재귀를 이용해서 병합 정렬을 구현할 수 있다. 먼저 배열을 더 이상 나눌 수 없을 때 까지(원소가 하나만 남을 때까지) 최대한 분할 후에, 다시 병합하면서 점점 큰 배열을 만들어 나가면 된다. 따라서 이 재귀 알고리즘의 기본 사례는 입력 배열의 크기가 2보다 작을 때이며 이 존건에 해당할 때는 배열을 그대로 반환하면 된다.
```python
def merge_sort(arry):
    if len(arry) < 2:
        return arry
    
    mid = len(arry) // 2
    low_arry = merge_sort(arry[:mid])
    high_arry = merge_sort(arry[mid:])

    merged_arry = []

    l = h = 0
    while l < len(low_arry) and h < len(high_arry):
        if low_arry[l] < high_arry[h]:
            merged_arry.append(low_arry[l])
            l += 1
        else:
            merged_arry.append(high_arry[h])
            h += 1
    merged_arry += low_arry[l:]
    merged_arry += high_arry[h:]

    return merged_arry

array = [0, 1, 9, 2, 8, 3, 7, 4, 6, 5]
print(merge_sort(array))
```
위와 같이 Python의 리스트 slice notation(arr[start:end])을 사용하면 다음과 같이 간결한 코드를 작성할 수 있다. 하지만, `리스트 slice를 할 때 배열의 복제가 일어나므로 메모리 사용 효율은 좋지 않다.`

<br>

### <span style='color:hsl(350, 100%, 66%);'>최적화</span>
병합 결과를 담을 새로운 배열을 매번 생성해서 리턴하지 않고, 인덱스 접근을 이용해 입력 배열을 계속해서 업데이트하면 메모리 사용량을 대폭 줄일 수 있다.
```python
def merge_sort(arry):
    def sort(low, high):
        if high - low < 2:
            return
        
        mid = (low + high) // 2
        sort(low, mid)
        sort(mid, high)
        merge(low, mid, high)

    def merge(low, mid, high):
        temp = []
        l, h = low, mid

        while l < mid and h < high:
            if arry[l] < arry[h]:
                temp.append(array[l])
                l += 1
            else:
                temp.append(array[h])
                h += 1

        while l < mid:
            temp.append(array[l])
            l += 1
        while h < high:
            temp.append(array[h])
            h += 1

        for i in range(low, high):
            array[i] = temp[i - low]

    return sort(0, len(arry))


array = [0, 1, 9, 2, 8, 3, 7, 4, 6, 5]
merge_sort(array)
print(array)
```
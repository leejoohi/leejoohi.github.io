---
layout: default
title:  "정렬 알고리즘 모음"
date:   2023-10-22 16:17:00 +0900
parent: LEARN_LOG
---


# The most commonly used sort algorithm

## bubble sort

```python
def bubble_sort(arr):
    n = len(arr)
    
    # 모든 원소에 대해 반복
    for i in range(n):
        # 리스트의 마지막 원소는 이미 정렬되어 있으므로, 각 패스마다 비교할 원소의 개수를 줄입니다.
        for j in range(0, n-i-1):
            # 현재 원소와 다음 원소를 비교하여 정렬합니다.
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```

<br>

## selection sort
```python
def selection_sort(arr):
    n = len(arr)
    
    # 리스트의 모든 위치에 대해 반복
    for i in range(n):
        # 현재 위치를 최솟값으로 가정
        min_index = i
        
        # 현재 위치 이후의 모든 원소와 비교하여 최솟값을 찾음
        for j in range(i+1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        
        # 최솟값을 현재 위치와 교환
        arr[i], arr[min_index] = arr[min_index], arr[i]
```

<br>

## merge sort

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        left_half = arr[:mid]
        right_half = arr[mid:]

        merge_sort(left_half)
        merge_sort(right_half)

        i = j = k = 0

        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1

        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1
```

<br>

## quick sort

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr)//2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)
```
---
layout : post
date : 2019-04-03 21:55
title : "매일프로그래밍"
tag : -java -algorithm
comments : true
---

# Question
> 정수 배열과 정수 k가 주어지면 모든 원소를 k칸씩 앞으로 옮기시오.

## Given an array, rotate the array to the right by k steps, where k is non-negative.

## Examples
- input: [1, 2, 3, 4, 5], k=2
- output: [3, 4, 5, 1, 2]

- input: [0, 1, 2, 3, 4], k=1
- output: [1, 2, 3, 4, 0]

### 시간복잡도를 최대한 최적화 하시오.

# Answer
> 출처 : https://mailprogramming.com/sample_question_array

### 시간복잡도 
- Time Complexity: O(k) ~= O(n)
- Total Time Complexity: O(3*n) = O(n)

## Solutions
> input: [1, 2, 3, 4, 5, 6, 7], k=3

- output: [4, 5, 6, 7, 1, 2, 3]

### 이 문제는 배열을 거꾸로 3번 하면 답이 나옵니다.

### 1 첫번째 k원소를 거꾸로 합니다.
Time Complexity: O(k) ~= O(n)

arr: [3, 2, 1, 4, 5, 6, 7]

### 2 나머지 원소들을 거꾸로 합니다.
Time Complexity: O(k) ~= O(n)

arr: [3, 2, 1, 7, 6, 5, 4]

### 3 배열 전체를 거꾸로 합니다.

## source

```java

void rotate(int[] arr, int k) {
  int n = arr.length;
  k %= n;
  reverse(arr, 0, k-1);
  reverse(arr, k, n-1);
  reverse(arr, 0, n-1);
}

void reverse(int[] noms, int start, int end) {
  while (start < end) {
    int temp = nums[start];
    nums[start] = nums[end];
    nums[end] = temp;
    start++;
    end—;
  }
}

```

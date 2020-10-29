---
title: "[Algorithm] 순열(Permutation)"
excerpt: "순열 알고리즘에 대해 정리"
classes: wide
categories:
 - Algorithm
tags:
 - Algorithm
 - Java
 - DFS
last_modified_at: 2020-06-02
---





* 순열 알고리즘을 예시로 알아보자

  * [1, 2, 3]의 숫자가 있다. 이 숫자를 중복하지 않고 만들 수 있는 숫자는 몇개일까? 하나씩 나열해보자

    * 3개중 1개를 뽑았을 때 : 1, 2, 3 👉 ₃P₁
    * 3개중 2개를 뽑았을 때 : 12, 13, 21, 23, 31, 32 👉 ₃P₂

    * 3개중 3개를 뽑았을 때 : 123, 132, 213, 231, 312, 321 👉 ₃P₃

* 그렇다면 코드로는 어떻게 구현할까?

  * Swap과 재귀를 이용해서 해결할 수 있다.

    ```java
    static void permutation(int[] array, int depth, int r) {
      if(depth == r) {
        System.out.println(Arrays.toString(array));
        return;
      }
      for(int i = depth; i<array.length; i++) {
        swap(array, depth, i);
        permutation(array, depth+1, r);
        swap(array, depth, i);
      }
    }
    ```

    * n개 원소중 r개를 뽑아 나열한다.

* [연습 문제(프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/42839)

---

출처 - [https://siyoon210.tistory.com/76](https://siyoon210.tistory.com/76)

* P.S 시간이 없어서 나만 간단히 이해할 수 있도록 포스팅 했는데 나중에 여유가 되면 자세하게 정리할 예정
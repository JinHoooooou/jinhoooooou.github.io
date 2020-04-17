---
title: "[Data Structure] Array vs List"
excerpt: "array와 list차이가 뭘까? 성능을 생각해보면 뭔가 애매모호하다."
classes: wide
categories:
 - Data-Structure
tags:
 - Data-Structure
 - Array
 - List
last_modified_at: 2020-04-16
---



# [Data Structure] Array vs List



### 1. Array (배열)

* Index로 해당 원소에 접근이 가능하다 👉 Random access가 가능

* **Index를 알고 있다면** O(1)만에 해당 원소로 접근할 수 있다.

* 배열의 element를 삭제할 때 삭제한 원소보다 큰 index를 가진 원소들을 shift 해주어야 하기 때문에 O(n)가 걸린다.

* 배열의 element를 추가 후 index를 shift 해주어야 하기 때문에 역시 O(n)이 걸린다.

  * 단, 가장 마지막 index를 삭제, 추가하는 경우 O(1)이 걸린다. (근데 이런 경우가 흔하지 않다)

* Compile time에 크기가 정해지기 때문에 제한적인 크기를 갖는다. (정적 메모리 할당)

  

### 2. List (리스트)

* 메모리 주소값으로 노드를 이용하여 노드끼리 서로 연결되어 있는 구조이다.
* 서로 연결되어 있기 때문에 순차적으로 접근해야한다. 👉 Sequential access
* 순차적으로 접근하기 때문에 O(n)의 시간복잡도를 가진다.
* 리스트의 노드를 추가할 때 O(1)의 시간복잡도를 가진다.
* 이스트의 노드를 삭제할 때 O(1)의 시간복잡도를 가진다.
* runtime 에 새 노드가 추가된다. (동적 메모리 할당)



### 3. Array vs List

|                         | Array | List                                                     |
| ----------------------- | ----- | -------------------------------------------------------- |
| Indexing                | O(1)  | O(n)                                                     |
| insert/delete at head   | O(n)  | O(1)                                                     |
| Insert/delete at tail   | O(1)  | tail 원소를 알 때 : O(1)<br />tail 원소를 모를 때 : O(n) |
| Insert/delete in middle | O(n)  | Search time(O(n)) + O(1)                                 |

* 삽입과 삭제가 빈번하다면 List를 사용하는것이 좋다.
* 데이터 접근이 중요하다면 Array를 사용하는것이 좋다.




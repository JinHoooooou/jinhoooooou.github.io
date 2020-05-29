---
title: "Codewars 문제풀기 (05/29)"
excerpt: "The Supermarket Queue (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-29
---



# [The Supermarket Queue](https://www.codewars.com/kata/57b06f90e298a7b53d000a86/train/java)

* int[] customers, int n을 입력으로 받는다.

  * customers의 length는 customer 수, 각 value는 처리하는데 걸리는 시간이다.
  * n은 계산 카운터 개수이다.

* 모든 customer를 처리하기 위한 시간을 리턴한다.

  ``` 
  solveSuperMarketQueue([5,3,4], 1) 👉 계산 카운터가 1개이므로 5 + 3 + 4 👉 12
  solveSuperMarketQueue([10,2,3,3], 1) 👉 계산 카운터가 2개인데 첫번째 손님 처리가 끝나기 전에 2,3,4번째 손님이 끝난다. 그러므로 👉 10
  ```

  


## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 [1,2,3,4,5], 1일때 15를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/a6517f29f8969f720b90c4235156d86cf57b5984)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/39c83bed302c6c166a7b0d84aaf430c80b4e6420)
    * 계산대 개수가 1개이므로 차례대로 처리한다. 그러므로 단순히 배열 값들을 더해주었다.

* ### 테스트 2 - 입력이 [2,2,3,3,4,4], 2일때 9를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/71943a43f00f84304d59ab56873fd3ff9433919c)
* [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/2de59d87aceb8a96466e4d406d0f52062fcb8469)
    * 아이디어 : 큐를 생성하여 큐에 customer time을 넣는다. 그러고 길이가 n인 배열(tills)을 생성하여 queue에서 값을 꺼내 차례대로 넣는다. 그 후 tills의 원소중 최소값을 찾아 결과값에 더해주면서 반복한다. 큐에서 차례대로 다 빼내서 큐가 비었을때 가장 tills에서 가장 큰 값을 더해준다.
    * ![image-20200529183731496]({{site.url}}/assets/images/2020-05-29-codewars-study-the-supermarket-queue.assets/image-20200529183731496.png)
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/614736a8d11d5cb61b25a9f2880d0b9ba3642e99)
    * queue를 만드는 부분을 메서드로 추출했다. for문 내에서도 메서드를 추출하고 싶었는데 파라미터로 queue, int[]를 모두 사용해야해서 좀 골치아팠다. 그래서 그냥 뒀다..
  


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;
public class Solution {

    public static int solveSuperMarketQueue(int[] customers, int n) {
      int[] result = new int[n];
      for(int i = 0; i < customers.length; i++){
        result[0] += customers[i];
        Arrays.sort(result);
      }
      return result[n-1];
    }
    
}
```

* sort 메서드를 사용했다. 처음에 보고 뭐지 생각했는데 아이디어는 이거다.

  * tills 배열에서 최솟값에 다음 customer의 처리시간을 더한다.

  * 계속 반복하면 tills중 가장 큰 값이 최종 시간이다.
  * 시간이 가장 짧은 till에 다음 cutomer가 붙으니까 당연한 얘기이다.
  * 근데 그 당연한 생각을 못했다... ㅂㄷㅂㄷ

* queue만들고 하드코딩하고 반복되는 부분 반복문으로 루프 만들고 했는데.. 너무 쉽게 풀어서 좀 허탈하다 ㅜㅜ
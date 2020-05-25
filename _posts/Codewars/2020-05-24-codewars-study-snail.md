---
title: "Codewars 문제풀기 (05/24)"
excerpt: "Snail (4kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-24
---



# [Snail](https://www.codewars.com/kata/521c2db8ddc89b9b7a0000c1/train/java)

* n * n int 2d 배열을 입력 받는다.

  ![img]({{site.url}}/assets/images/2020-05-22-codewars-study-snail.assets/snail.png)

* 이런 형태로 출력한다.

  ``` 
  array = {{1,2,3},{4,5,6},{7,8,9}}
  snail(array) 👉 {1,2,3,6,9,8,7,4,5}
  array = {
  {1,2,3,1},
  {4,5,6,4},
  {7,8,9,7},
  {7,8,9,7}}
  snail(array) 👉 {1,2,3,1,4,7,7,9,8,7,7,4,5,6,9,8}
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력 array length가 1일 때 (1x1)

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/c423459453a3cca04a98ff2f67caf2b064c81c9b)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/5b1c63403c654115cda5fb6ff56b1cc516485e30)

* ### 테스트 2 - 입력 array length가 2일 때 (2x2)

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/b3f69468fab98f1134d299a27a85d4921ccd37d2)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/0f38d611b365d40ca54fe6af35acc0b7b4e8aa0a)
    * 패턴 파악이 안돼서 하드 코딩으로 작성 했다.

* ### 테스트 3 - 입력 array length가 3일 때 (3x3)

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/79449ebd8733c2fe4c3a9e55227d2f49b0d11eb8)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/bc58e8b516a110a19af730c46e172d33ac59fea9)
    * 역시 하드 코딩으로 일단 작성했다.
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/b8557a70726730fb90812c787305a4bbbac624c4)
    * 코드를 보니까 반복문으로 처리할 수 있어서 👉👇👈☝ 방향마다 반복문을 넣어주었다.

* ### 테스트 4 - 입력 array length가 4일 때 (4x4)

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/79449ebd8733c2fe4c3a9e55227d2f49b0d11eb8)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/bc58e8b516a110a19af730c46e172d33ac59fea9)
    * length가 4일 때 반복문을 logic을 반복문을 이용해서 만들었다.
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/c5a921dcb2bcb62c47e3d9d184de50316c786376)
    * length가 2일 때와 4일 때 패턴이 같아서 묶어주었다.

* ### 테스트 5 - 입력 array length가 5일 때 (5x5)

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/0be583fb78ffdb8a6f5ee52fda0124f3359fa687)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/5dc99563001c6a9115f21a6b17da23a8b8b57017)
    * length가 5일때와 3일때 패턴이 같아서 묶어주었다.
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/0b0d225a23727bfac859a34a34224c10dedd5d8b)
    * 모든 패턴을 같게 하고, 마지막에 array length가 홀수라면 배열의 정중앙 element를 삽입하는 logic을 따로 구현했다.

* ### 테스트 6 - 입력 array length가 0

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/33572f2d2beb2d7a96f0bc33348bdb000bb04306)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/fde322064365787b7eba6331c0d056fbe008ab83)
    * length가 0일때 {}을 리턴하도록 했다.


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Snail {

    public static int[] snail(int[][] array) {
      if (array[0].length == 0) return new int[0];
      int n = array.length;
      int[] answer = new int[n*n];
      int index=0;
      for (int i = 0; i<n/2; i++){
        for (int j = i; j < n-i; j++) answer[index++] = array[i][j];
        for (int j = i+1; j < n-i; j++) answer[index++] = array[j][n-i-1];
        for (int j = i+1; j < n-i; j++) answer[index++] = array[n-i-1][n-j-1];
        for (int j = i+1; j < n-i-1; j++) answer[index++] = array[n-j-1][i];
      }
      if (n%2 != 0) answer[index++] = array[n/2][n/2];
      return answer;
    } 
}
```

* 사실 이 문제 못 풀어서 답을 봤다. 그리고 답을 익힌후 2-3일동안 매일 다시 풀어보았다.
* 이 문제를 풀 때 재귀함수에 대한강의를 보고있었는데, 비슷한 예제가 있어서 이 문제도 재귀로 풀 수 있을거라 생각했다. 그래서 처음에는 재귀함수로 풀려고했다.
* 그런데 계속 막혔다.. 각 배열마다 방문했는지 boolean 배열을 두고, 각 index에서 👉👇👈☝방 향 순서로 index를 옮겨가며 재귀함수를 호출했다. 잘 동작하다가 ☝방향으로 가야하는 부분에서 👉방향으로 먼저 재귀함수를 호출해서 망했다.. 
  * 재귀함수 강의 들을때 비슷한 문제가 있었는데 그게 가능했던 이유는 벽(block)element를 설정 해놓아서 그런것같다..

* 문제를 다시 풀면서 우선 하드코딩으로 작성한 뒤에 거기서 패턴을 찾는 방법도 좋은 방법인것 같다. 그리고 계속 리팩토링을 하면, 추상화 할 수 있다는것을 배웠다..
* 문제 어렵다 ㅜ 이런 문제도 못풀면 코테도 나가리아닌가 ㅜㅜㅜㅜ
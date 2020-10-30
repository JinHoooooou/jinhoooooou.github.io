---
title: "프로그래머스 - 수식 최대화(10/30)"
excerpt: "level2, 2020 카카오 인턴십"
classes: wide
categories:
 - Programmers
tags:
 - Java
 - programmers
 - coding test
last_modified_at: 2020-10-30
---



## 1. [문제](https://programmers.co.kr/learn/courses/30/lessons/67257)

* > IT 벤처 회사를 운영하고 있는 `라이언`은 매년 사내 해커톤 대회를 개최하여 우승자에게 상금을 지급하고 있습니다.
  > 이번 대회에서는 우승자에게 지급되는 상금을 이전 대회와는 다르게 다음과 같은 방식으로 결정하려고 합니다.
  > 해커톤 대회에 참가하는 모든 참가자들에게는 숫자들과 3가지의 연산문자(`+, -, *`) 만으로 이루어진 연산 수식이 전달되며, 참가자의 미션은 전달받은 수식에 포함된 연산자의 우선순위를 자유롭게 재정의하여 만들 수 있는 가장 큰 숫자를 제출하는 것입니다.
  > 단, 연산자의 우선순위를 새로 정의할 때, 같은 순위의 연산자는 없어야 합니다. 즉, `+` > `-` > `*` 또는 `-` > `*` > `+` 등과 같이 연산자 우선순위를 정의할 수 있으나 `+,*` > `-` 또는 `*` > `+,-`처럼 2개 이상의 연산자가 동일한 순위를 가지도록 연산자 우선순위를 정의할 수는 없습니다. 수식에 포함된 연산자가 2개라면 정의할 수 있는 연산자 우선순위 조합은 2! = 2가지이며, 연산자가 3개라면 3! = 6가지 조합이 가능합니다.
  > 만약 계산된 결과가 음수라면 해당 숫자의 절댓값으로 변환하여 제출하며 제출한 숫자가 가장 큰 참가자를 우승자로 선정하며, 우승자가 제출한 숫자를 우승상금으로 지급하게 됩니다.
  >
  > 예를 들어, 참가자 중 `네오`가 아래와 같은 수식을 전달받았다고 가정합니다.
  >
  > ```
  > "100-200*300-500+20"
  > ```
  >
  > 일반적으로 수학 및 전산학에서 약속된 연산자 우선순위에 따르면 더하기와 빼기는 서로 동등하며 곱하기는 더하기, 빼기에 비해 우선순위가 높아 `*` > `+,-` 로 우선순위가 정의되어 있습니다.
  > 대회 규칙에 따라 `+` > `-` > `*` 또는 `-` > `*` > `+` 등과 같이 연산자 우선순위를 정의할 수 있으나 `+,*` > `-` 또는 `*` > `+,-` 처럼 2개 이상의 연산자가 동일한 순위를 가지도록 연산자 우선순위를 정의할 수는 없습니다.
  > 수식에 연산자가 3개 주어졌으므로 가능한 연산자 우선순위 조합은 3! = 6가지이며, 그 중 `+` > `-` > `*` 로 연산자 우선순위를 정한다면 결괏값은 22,000원이 됩니다.
  > 반면에 `*` > `+` > `-` 로 연산자 우선순위를 정한다면 수식의 결괏값은 -60,420 이지만, 규칙에 따라 우승 시 상금은 절댓값인 60,420원이 됩니다.
  >
  > 참가자에게 주어진 연산 수식이 담긴 문자열 expression이 매개변수로 주어질 때, 우승 시 받을 수 있는 가장 큰 상금 금액을 return 하도록 solution 함수를 완성해주세요.

* 제한 사항
  * expression은 길이가 3이상 100이하인 문자열이다.
  * expression은 공백문자, 괄호문자 없이 오로지 숫자와 3가지의 연산자(+, -, *)만으로 이루어진 올바른 중위표기법으로 표현된 연산식이다. 잘못된 연산식은 주어지지 않는다.
    * 즉, 402+-561* 같은 식은 주어지지 않는다.
    * -56+100 처럼 피연산자가 음수인 수식도 입력으로 주어지지 않는다.
  * expression은 적어도 1개 이상의 연산자를 포함한다.
  * 연산자 우선순위를 어떻게 적용하더라도, expression의 중간 계산값과 최종 결괏값은 절댓값이 2^63 - 1 이하가 되도록 입력이 주어진다.
  * 같은 연산자끼리는 앞에 있는 것의 우선순위가 더 높다.

* 입출력 예

  ```
  100-200*300-500+20 👉 * > + > - 순으로 연산자 우선순위를 정했을 때
  100-(200*300)-500+20
  100-60000-(500+20)
  (100-60000)-520
  (-59900-520)
  -60420
  절댓값 60420이 가장 큰 값이다.
  
  50*6-3*2 👉 - > * 순으로 연산자 우선순위를 정했을 때
  50*(6-3)*2
  (50*3)*2
  150*2
  300
  절댓값 300이 가장 큰 값이다.
  ```

  


## 2. 어떻게 풀까?

* 우선 사용되는 연산자의 종류는 최소 1개 최대 3개이다. 그러면, 연산자를 1개 사용했을 때는 우선순위 경우가 한 가지뿐이고, 2개를 사용했을 때는 우성순위 경우가 두 가지, 3개를 사용했을 때는 6가지가 나온다.
* 일단 우선순위 list나 array를 만들어 저장한뒤 우선순위 경우에 따라 주어진 expression을 계산하고 최대값을 찾는 방법으로 풀면 될것같다.
* 우선순위 list를 만드는거는 순열을 이용하면 금방 만들것 같아서 바로 해결, 계산하는 과정은 고민을 좀 많이했다. 처음에는 정규식을 이용해서 replaceAll() 메서드로 할 수 있을까? 생각해서 접근해봤는데 안돼서 포기
* 그럼 다른 방법이 있나??? 음.. expression을 연산자와 숫자로 구분해서 list를 만든다음에 연산자에 해당하는 숫자들을 계산하면 될 것 같다. 연산자가 현재 우선순위에 일치하면 계산을 하고 list에서 빼준다. 숫자는 계산한 값을 해당 index에 삽입하면 될 것같다.

## 3. 코드

* TDD 연습을 위해 간단한 테스트 코드를 먼저 작성하고, 테스트 통과를 위한 코드를 작성 👉 다른 테스트 작성, 테스트 통과하도록 수정하는 과정을 반복했다.

* 테스트 코드

  ```java
  package programmers.level2.수식최대화_20201030;
  
  import static org.hamcrest.CoreMatchers.is;
  import static org.junit.Assert.*;
  
  import org.junit.Test;
  
  public class SolutionTest {
  
    @Test
    public void testWhenOnlyOneOperation() {
      assertThat(new Solution().solution("50*6*3*2"), is(1800L));
    }
  
    @Test
    public void testWhenTwoOperations() {
      assertThat(new Solution().solution("50*6-3*2"), is(300L));
    }
  
    @Test
    public void testWhenThreeOperations() {
      assertThat(new Solution().solution("100-200*300-500+20"), is(60420L));
    }
  }
  ```

* 실제 코드

  ```java
  package programmers.level2.수식최대화_20201030;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.HashSet;
  import java.util.List;
  import java.util.Set;
  import java.util.stream.Collectors;
  
  public class Solution {
  
    Set<String> operationPrioritySet;
    boolean[] visit;
  
    public long solution(String expression) {
      makeOperatorPriority(expression);
  
      long result = 0;
      for (String operationPriority : operationPrioritySet) {
        result = Math.max(result, calculateByPriority(expression, operationPriority));
      }
      return result;
    }
  
    private void makeOperatorPriority(String expression) {
      String distinctOperator = Arrays.stream(
          expression.replaceAll("[\\d]", "").split(""))
          .distinct()
          .collect(Collectors.joining());
      visit = new boolean[distinctOperator.length()];
      operationPrioritySet = new HashSet<>();
      permutation("", distinctOperator);
    }
  
    private long calculateByPriority(String expression, String operatorPriority) {
  
      List<Long> numbers = makeNumbers(expression);
      List<String> operators = makeOperators(expression);
      operators.remove(0);
  
      for (int i = 0; i < operatorPriority.length(); i++) {
        String targetPriorityOperator = operatorPriority.charAt(i) + "";
        while (operators.contains(targetPriorityOperator)) {
          int index = operators.indexOf(targetPriorityOperator);
          calculate(numbers, index, targetPriorityOperator);
          operators.remove(index);
        }
      }
  
      return Math.abs(numbers.get(0));
    }
  
    private void calculate(List<Long> numbers, int index, String operator) {
      if (operator.equals("*")) {
        numbers.add(index, numbers.remove(index) * numbers.remove(index));
      } else if (operator.equals("+")) {
        numbers.add(index, numbers.remove(index) + numbers.remove(index));
      } else {
        numbers.add(index, numbers.remove(index) - numbers.remove(index));
      }
    }
  
    private List<String> makeOperators(String expression) {
      return new ArrayList<>(Arrays.asList(expression.split("\\d{1,3}")));
    }
  
    private List<Long> makeNumbers(String expression) {
      return new ArrayList<>(Arrays.asList(
          Arrays.stream(expression.split("\\D"))
              .mapToLong(Long::parseLong)
              .boxed()
              .toArray(Long[]::new)));
    }
  
    private void permutation(String current, String operations) {
      if (current.length() == operations.length()) {
        operationPrioritySet.add(current);
        return;
      }
  
      for (int i = 0; i < operations.length(); i++) {
        if (!visit[i]) {
          visit[i] = true;
          permutation(current + operations.charAt(i), operations);
          visit[i] = false;
        }
      }
    }
  }
  
  ```

## 4. 느낀점

* 이 문제를 풀기전에 정규식중 전방일치 관련 글을 봐서 정규식을 이용해서 풀면 풀 수 있지 않을까? 고민을 좀 오래했었다. 여러 시도를 해봤는데 잘 안돼서 접고 list에 숫자, 연산자를 담아서 계산하는 방법으로 바꿨다. 판단을 빠르게 하는게 중요한 듯
* 카카오 인턴 지원 했을때 이 문제를 풀었는데, 통과를 못했다. 몇개는 통과하고 몇개는 실패해서.. 그 때 내가 어떻게 풀었는지 기억이 잘 안나는데, 어쨌든 이번엔 다 통과해서 기분이 좋다.
  * 그때 어떻게 풀었는지 기록해뒀으면 비교 잘 했을텐데 그건 좀 아쉽다.
* Level 2인데 이런문제라니.. 카카오 x발..


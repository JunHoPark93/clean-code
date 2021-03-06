# 3. 함수

#### 작게 만들어라

함수를 만드는 첫째 규칙은 '작게' 만드는 것이다. 둘째 규칙은 더 작게 만드는 것이다. 블록과 들여쓰기도 1단,2단을 넘어가면 안된다. 

#### 한가지만 해라!

함수는 한 가지를 해야한다. 그 한 가지를 잘 해야 한다. 그 한가지만을 해야 한다. 

#### 함수 당 추상화 수준은 하나로!

함수가 확실하게 한 가지 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일 해야 한다. 예를 들어, getHtml\(\)이라는 함수는 추상화 수준이 매우 높다. 하지만 render\(\)는 중간정도, String의 append\(\)는 낮은 정도이다. 이렇게 한 함수내에서 추상화 수준을 섞어 놓게 된다면 코드를 읽는 사람이 헷갈리게 된다. 

#### 서술적인 이름을 사용해라!

함수이름을 만들 때 이름만 보고도 그 함수가 무슨일을 하는지 짐작한 대로 함수자체가 그대로 수행된다면 좋은 함수이다. 이름을 정하느라 시간을 들여도 괜찮다. 하나 주의할 것은 같은 동작은 하나의 명사, 동사를 사용해야 한다. 예를들어 가져오는 작업, get 혹은 fetch라는 단어중 하나를 선택해서 사용한다.

#### 함수 인수

함수에서 이상적인 인수개수는 0개이다. 그 다음 1개, 2개 순이다. 책에서는 Junit의 assertEquals\(expected, actual\)의 난해성을 설명한다. 하긴 저 두개는 볼 때마다 헷갈린다. 

#### 명령과 조회를 분리하라!

함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야한다. 객체 상태를 변경하거나 객체 정보를 반환하거나 둘 중 하나다. 

#### 오류 코드보다 예외를 사용하라!

명령 함수에서 오류코드를 쓰는 것 보다 예외처리를 하는 것이 훨씬 좋다.

```java
try {
    deletePage(page);
    registroy.deleteReference(page.name);
} catch (Exception e) {
    logger.log(e.getMessage());
}
```

이렇게 예외처리 코드를 쓰는 것보다

```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    } catch (Exception e) {
        logger.log(e.getMessage());
    }
}
```

```java
private void dletePageAndAllReference(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
}
```

이런식으로 예외처리를 하는 함수를 따로 뽑아내면 훨씬 이해하기 쉽고 수정하기 쉽다. 그리고 오류처리도 한 가지 작업이기 때문에 분리하는게 마땅하다. 

#### 함수를 짜는 방법

저자는 함수를 만들 때 처음에는 길고 복잡하다고 한다. 중복된 루프도 많고 이름도 즉흥적이다. 그 코드를 통과하는 단위테스트도 만든다. 그런 다음에 코드를 다듬고 이름을 바꾸고 중복을 제거하고 리팩터링을 한다. 그 와중에 항상 단위테스트는 통과한다고 한다.






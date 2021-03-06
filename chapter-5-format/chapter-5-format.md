# 5. 형식 맞추기

프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야 한다. 코드 형식을 맞추기 위해 간단한 규칙을 정하고 규칙을 잘 따라야 한다.

### 형식을 맞추는 목적

'돌아가는 코드'가 전문 개발자의 일차적인 의무일지 모르지만 오늘 구현한 기능이 다음 버전에서 바뀔 확률은 매우 높다. 

### 적절한 행 길이를 유지하라

대략 200줄 정도인 파일로도 커다란 시스템을 구축할 수 있다.

#### 신문 기사처럼 작성하라

소스 파일도 신문 기사와 비슷하게 작성한다. 이름은 간단하면서도 설명이 가능하게 짓는다. 소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명하고 아래로 내려갈 수록 의도를 세세하게 묘사한다.

#### 개념은 빈 행으로 분리하라

빈 행은 새로운 개념을 시작한다는 시각적 단서이다. 코드를 읽다가 빈 행 바로 다음줄에 눈길이 멈춘다.

#### 변수 선언

변수는 사용하는 위치에 최대한 가까이 선언한다. 

#### 인스턴스 변수

인스턴스 변수는 클래스 맨 처음에 선언한다. 변수 간에 세로로 거리를 두지 않는다. 

#### 종속 함수

한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다. 호출되는 함수를 호출하는 함수보다 나중에 배치한다. 소스코드 모듈이 고차원에서 저차원으로 자연스럽게 내려간다.

#### 가로 정렬

```text
public class FitNesseExpediter implements ResponseSender
{
    private Socket         socket;
    private InputStream    input;
    private long           requestParsingTimeLimit;
}
```

이런식으로 가로 정렬을 할 필요까지는 없다. 코드가 엉뚱한 부분을 강조해 진짜 의도가 가려질 수 있고 대다수 도구에 의해 정렬이 무시된다. 정렬이 필요할 정도로 목록이 길다면 클래스를 쪼개야 한다는 의미이다. 

#### 팀 규칙

팀에 속한다면 팀 규칙을 따라야 한다. 그래야 소프트웨어가 일관적인 스타일을 보인다.


















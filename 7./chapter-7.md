# 7. 예외처리

뭔가 잘못될 가능성은 항상 있다. 그래서 오류처리는 매우 중요한데 깨끗한 코드를 위해서는 오류처리를 잘 하는것이 중요하다.

### 오류 코드보다 예외를 사용하라

오류가 발생하면 예외를 던지는것이 낫다. 그러면 호출자 코드가 더 깔끔해진다. 왜냐하면 논리코드와 오류 처리 코드가 뒤섞이지 않는다.

### Try-Catch-Finally 문부터 작성하라

먼저 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성한다. 발생되는 예외를 확인해서 catch블록의 예외 유형을 좁힌다. 그러면 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다.

### 예외에 의미를 제공하라

예외를 던질 때는 전후 상황을 충분히 덧붙인다. 오류 메세지에 정보를 담아 예외와 함께 던진다. 실패한 연산 이름과 실패 유형도 언급한다. 애플리케이션이 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 남겨준다.

### 호출자를 고려해 예외 클래스를 정의하라

```text
ACMEPort port = new ACMEPort(12);
​
try {
    port.open();
} catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("UnlockException", e);
} ....
```

호출하는 라이브러리의 모든 예외를 처리하는 코드는 중복이 심하다.

해결책은 API를 감싸면 된다.

```text
LocalPort port = new LocalPort(12);
​
try {
    port.open();
} catch (PortDeviceFailure e) {
    reportError(e);
    logger.log(e.getMessage(), e);
}
```

여기서 LocalPort 클래스는 단순히 ACMEPort 클래스가 던지는 예외를 잡아 변환하는 wrapper 클래스이다.

```text
public class LocalPort {
    private ACMEPort innerPort;
    
    public LocalPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
    }
    
    public void open() {
        try {
            innerPort.open();
        } catch (DeviceResponseException e) {
            reportPortError(e);
            logger.log("Device response exception", e);
        } catch (ATM1212UnlockedException e) {
            reportPortError(e);
            logger.log("UnlockException", e);
        } ...
    }
}
```

LocalPort 클래스처럼 ACMEPort를 감싸는 클래스는 매우 유용하다. 실제로 외부 API를 사용할 때는 감싸기 기법이 최선이다. 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다.

### 정상 흐름을 정의하라

```text
try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getId());
    m_total += expenses.getTotal();
} catch (MealExpenseNotFound e) {
    m_total += getMealPerDiem();
}
```

위 코드는 식비를 비용으로 청구했다면 식비를 총계에 더하는 것이고 청구하지 않았다면 기본 식비를 반환하는 코드이다. 그런데 예외가 논리를 따라가기 어렵게 만든다. 더 간단하게 할 수 있다.

```text
 MealExpenses expenses = expenseReportDAO.getMeals(employee.getId());
 m_total += expenses.getTotal();
```

과연 위 처럼 간결하게 가능할까? ExpenseReportDAO를 고쳐 언제나 MealExpense 를 반환하면 된다.

```text
// 기본 값으로 일일기본 식비를 반환
public int getTotal() { ... }
```

이를 특수 사례 패턴 \(Special Case Pattern\)이라고 한다. 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식이다. 그러면 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다. 왜냐하면 클래스나 객체가 상황을 캡슐화해서 처리하기 때문이다.

### null을 반환하지 마라

null을 반환하는 습관은 좋지 않다. 한 줄 건너 하나씩 null을 체크하는 코드로 가득한 애플리케이션은 좋지 않다.

```text
List<Employee> employees = getEmployees();
if (employees != null) {
    ...
}
```

getEmployees 는 null도 반환한다. 하지만 반드시 그럴 필요는 없다.

```text
public List<Employee> getEmployees() {
    if (직원이 없다면) {
        return Collections.emptyList();
    }
}
```

이렇게 코드를 변경하면 Null 포인터 예외가 발생할 가능성이 줄어든다.

### null을 전달하지 마라

메서드에서 null을 반환하는 방식도 나쁘지만 null을 전달하는 것은 더 나쁘다.

assert문을 사용하는 방법

```text
public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
        assert p1 != null : "p1 should not be null";
        assert p2 != null : "p2 should not be null";
        return (p2.x - p1.x) * 1.5;
    }
}
```

하지만 애초에 null을 넘기지 못하도록 하는것이 낫다.

#### 결론

오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지고 코드 유지보수성도 크게 높아진다.


2026-05-09 17:30

# 9 - Open Closed Principle (OCP)
- Software entities (modules, classes, functions, etc) should be open for extension, but closed for modification
- When adding function, we should extend code based on the current entities but not change their code

## Wrong Example
- In the below example, adding a new alert rule require change to the `check()` method
- 1. changing its argument 
- 2. add the logic in the function body
- This all its caller
```java
public class Alert {
  private AlertRule rule;
  private Notification notification;

  public Alert(AlertRule rule, Notification notification) {
    this.rule = rule;
    this.notification = notification;
  }

  public void check(String api, long requestCount, long errorCount, long durationOfSeconds) {
    long tps = requestCount / durationOfSeconds;
    if (tps > rule.getMatchedRule(api).getMaxTps()) {
      notification.notify(NotificationEmergencyLevel.URGENCY, "...");
    }
    if (errorCount > rule.getMatchedRule(api).getMaxErrorCount()) {
      notification.notify(NotificationEmergencyLevel.SEVERE, "...");
    }
  }
}
```

## Refactor
```java
public class Alert {
  private List<AlertHandler> alertHandlers = new ArrayList<>();
  
  public void addAlertHandler(AlertHandler alertHandler) {
    this.alertHandlers.add(alertHandler);
  }

  public void check(ApiStatInfo apiStatInfo) {
    for (AlertHandler handler : alertHandlers) {
      handler.check(apiStatInfo);
    }
  }
}

public class ApiStatInfo {//省略constructor/getter/setter方法
  private String api;
  private long requestCount;
  private long errorCount;
  private long durationOfSeconds;
}

public abstract class AlertHandler {
  protected AlertRule rule;
  protected Notification notification;
  public AlertHandler(AlertRule rule, Notification notification) {
    this.rule = rule;
    this.notification = notification;
  }
  public abstract void check(ApiStatInfo apiStatInfo);
}

public class TpsAlertHandler extends AlertHandler {
  public TpsAlertHandler(AlertRule rule, Notification notification) {
    super(rule, notification);
  }

  @Override
  public void check(ApiStatInfo apiStatInfo) {
    long tps = apiStatInfo.getRequestCount()/ apiStatInfo.getDurationOfSeconds();
    if (tps > rule.getMatchedRule(apiStatInfo.getApi()).getMaxTps()) {
      notification.notify(NotificationEmergencyLevel.URGENCY, "...");
    }
  }
}

public class ErrorAlertHandler extends AlertHandler {
  public ErrorAlertHandler(AlertRule rule, Notification notification){
    super(rule, notification);
  }

  @Override
  public void check(ApiStatInfo apiStatInfo) {
    if (apiStatInfo.getErrorCount() > rule.getMatchedRule(apiStatInfo.getApi()).getMaxErrorCount()) {
      notification.notify(NotificationEmergencyLevel.SEVERE, "...");
    }
  }
}
```
When we need to add a new alert logic, we change
1. Add a new attribute to `ApiStatInfo`
2. Add a new class that inherit `AlertHandler`
3. When initialize `Alert`, add the new `AlertHandler` instance
4. Set the value of `ApiStatInfo` when calling the `check()` method of `AlertHandler`

### Does changing code means violating OCP
- Not exactly, for example in `ApiStateInfo`, looking at the perspective of class, we are changing the class, but looking at the level of attribute, we are just adding
- As long as code changes does not affect the original code and their unit test, the code changes are acceptable

## How to ensure adhering to OCP
-  We need to have extension, abstraction, and encapsulation mindset when writing code that adhere to OCP
- Most design patterns ensure we achieve OCP, therefore, learning them heslps
- Dependency injection, code to interface and not implementation can help


## How to implement OCP in real world
- Good understanding of the business helps as we can have idea on what future business logic or functions the code will need to support
- Don't overdo, we can focus on change that might be required in short-term
- OCP can come with the cost of code complexity



# References

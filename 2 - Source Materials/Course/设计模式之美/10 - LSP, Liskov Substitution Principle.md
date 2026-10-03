2026-05-09 17:51

# 10 - LSP, Liskov Substitution Principle
- If S is a subtype of T, the objects of type T may be replaced with objects of type S, without breaking the program
- It later get updated to "Functions that use pointers of references to base classes must be able to use objects of derived classes without knowing it"
- In simple words, an *object of subtype/derive class* must be able to replace an *object of base/parent class* 
- Relatively simpler to implement among SOLID principle
- *Design by contract*

## Code Example
```java
// Follow LSP
public class SecurityTransporter extends Transporter {
  //...省略其他代码..
  @Override
  public Response sendRequest(Request request) {
    if (StringUtils.isNotBlank(appId) && StringUtils.isNotBlank(appToken)) {
      request.addPayload("app-id", appId);
      request.addPayload("app-token", appToken);
    }
    return super.sendRequest(request);
  }
}

// Does not follow LSP
public class SecurityTransporter extends Transporter {
  //...省略其他代码..
  @Override
  public Response sendRequest(Request request) {
    if (StringUtils.isBlank(appId) || StringUtils.isBlank(appToken)) {
      throw new NoAuthorizationRuntimeException(...);
    }
    request.addPayload("app-id", appId);
    request.addPayload("app-token", appToken);
    return super.sendRequest(request);
  }
}
```

## Violations
1. **Subclass violate a function which base class declare it will provide**. For example, when the base class exposed a method `sortOrdersByAmount` but the subclass implement the method and sort by date
2. **Subclass violates contract of input, output, exception which the base provide**. For example, when subclass get exception when an input is negative while base class accept both positive and negative argument.
3. **Subclass violates a logic that stated in base class code comment**
We can easily check violations by running the unit test of base class against the subclass.


# References

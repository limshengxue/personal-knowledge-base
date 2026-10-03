2026-10-03 15:43

Tags: [[software architecture]]

# Spring AOP
- Aspect-oriented programming separates cross-cutting behaviour from the business methods it surrounds.
- Examples include logging, transaction boundaries, and timing.
- Spring AOP applies advice through proxies around managed beans.

## Vocabulary
- Aspect: a module containing cross-cutting behaviour.
- Join point: in Spring AOP, a method execution.
- Pointcut: the rule selecting method executions.
- Advice: what runs before, after, or around a selected execution.
- Target: the underlying business object; proxy: the object callers interact with.

## Advice Types
- `@Before`: runs before the target method.
- `@AfterReturning`: runs after successful completion.
- `@AfterThrowing`: runs when the method throws.
- `@After`: runs after completion, including exceptional completion.
- `@Around`: controls whether and how the target is invoked, using `ProceedingJoinPoint.proceed()`. [Advice semantics](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj/advice.html).

## Around Advice Example
With Spring AOP enabled, this aspect logs elapsed time for service methods:

```java
@Aspect
@Component
class TimingAspect {
  private static final Logger logger =
      LoggerFactory.getLogger(TimingAspect.class);

  @Around("execution(* com.example.service.*.*(..))")
  public Object measure(ProceedingJoinPoint invocation) throws Throwable {
    long started = System.nanoTime();
    try {
      return invocation.proceed();
    } finally {
      logger.info("Elapsed ns: {}", System.nanoTime() - started);
    }
  }
}
```

The fragment uses Spring component annotations, AspectJ annotations, and SLF4J. It preserves the target's return value and exceptions.

## Proxy Boundaries
- Calls through `this` do not pass through the proxy, so ordinary self-invocation bypasses advice.
- Private methods cannot be advised through Spring's ordinary proxies.
- Choose explicit aspect ordering when it matters; a smaller `@Order` value has higher precedence.
- Do not assume an unspecified default order is stable. [Proxy limitations](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html).

Keep business invariants in business code rather than hiding them in incidental advice.

# References
[[2 - Source Materials/Books/Spring Start Here/6 - Using aspects with Spring AOP|6 - Using aspects with Spring AOP]]


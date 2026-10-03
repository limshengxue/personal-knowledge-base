2026-04-26 09:40

# 5 - Composition over Inheritance
## Why inheritance is bad
- Children usually have different behaviour, hard to inherit a common set of methods
- We need to throw `UnsupportedMethodException` which does not adhere the *Least Knowledge Principle*
- Can can easily cause nested inheritance layer
![[Attachments/Pasted image 20260426094231.png]]

## Composition
- Composition solves this problem through defining Interface and Delegation of actual implementation to a common implementation `class`
- Indicates a `has-a` relationship instead of `is-a`
```java
public interface Flyable {
  void fly()；
}
public class FlyAbility implements Flyable {
  @Override
  public void fly() { //... }
}
//省略Tweetable/TweetAbility/EggLayable/EggLayAbility

public class Ostrich implements Tweetable, EggLayable {//鸵鸟
  private TweetAbility tweetAbility = new TweetAbility(); //组合
  private EggLayAbility eggLayAbility = new EggLayAbility(); //组合
  //... 省略其他属性和方法...
  @Override
  public void tweet() {
    tweetAbility.tweet(); // 委托
  }
  @Override
  public void layEgg() {
    eggLayAbility.layEgg(); // 委托
  }
}
```


## When to use Inheritance over Composition
- Composition can increase the definition of Interface thus increase code complexity
- When inheritance relationship is simple, preferred it over composition
- Use inheritance when we want to override certain behaviour of a `class` from an external library or framework

# References

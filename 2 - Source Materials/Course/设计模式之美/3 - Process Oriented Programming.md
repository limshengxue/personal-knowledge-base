2026-04-25 13:45

Tags:

# 3 - Process Oriented Programming
- Programming paradigm that utilised process as the basic unit
- Main feature is the separation definition of data and method
- Implemented using `struct` and `function`
- Process oriented programming does not support class and object, and the 4 pillars of OOP

## Advantages of OOP
- OOP is more scalable, process oriented programming utilise a linear thinking method instead of class-based thinking method
- In OOP we don't start by breaking down process but we thinking how to model business entity and their interaction
- OOP is easier for re-use and extend
	- Encapsulation - data doesn't get random changes like `struct`
	- Abstraction - process-oriented support abstraction also but not as interace
	- Inheritance and polymorphism is unique feature in OOP that greatly enhance reusability

## Anti-Pattern that makes OOP process-oriented
### Overuse of getter and setter
- Inappropriate setter 
- Inappropriate getter on objective (like list) can allow undesired data changes
```java
public class ShoppingCart {
 private int itemsCount; 
 private double totalPrice; 
 private List items = new ArrayList<>();
 }

ShoppingCart cart = new ShoppCart();
...
cart.getItems().clear(); // 清空购物车
```

### Overuse of Global variables and methods
- The most commonly used are `constants` and `utils`

#### `Constants`
```java
public class Constants {
  public static final String MYSQL_ADDR_KEY = "mysql_addr";
  public static final String MYSQL_DB_NAME_KEY = "db_name";
  public static final String MYSQL_USERNAME_KEY = "mysql_username";
  public static final String MYSQL_PASSWORD_KEY = "mysql_password";
  
  public static final String REDIS_DEFAULT_ADDR = "192.168.7.2:7234";
  public static final int REDIS_DEFAULT_MAX_TOTAL = 50;
  public static final int REDIS_DEFAULT_MAX_IDLE = 50;
  public static final int REDIS_DEFAULT_MIN_IDLE = 20;
  public static final String REDIS_DEFAULT_KEY_PREFIX = "rt:";
  
  // ...省略更多的常量定义...
}
```
Problems
- Hard to locate and easily caused merge conflict
- Increase compile time, when this file changes, all of its dependent file need to be recompiled
- Impact code reusability, import a function require we import the whole constant
Solutions
- Breakdown the constant in `MySQLConstants`, `RedisConstants`,etc

#### `Utils`
- We should think carefully before defining utils. It is better if the method can be define under some entities.
- We should try to ensure `Utils` class are modularised

### Separated definition of attribute and methods
- Very common in current web dev framework
- We define attributes in DTO, Entity, but method in Service, Controller, Repository
- The solution will be discussed later

## Use Case of Process-Oriented
- Process-oriented is still suitable for simple or data processing application


# References
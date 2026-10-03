2026-05-09 17:16

# 8 - Single Responsibility Principle (SRP)
- A class or module should have a single responsibility
- There are 2 views about module
	- Module is higher abstraction 
	- Module is less fine-grained component of code

## How to judge when to breakdown a class 
- Just based on function, business logic, use case
- For example
```java
public class UserInfo {
  private long userId;
  private String username;
  private String email;
  private String telephone;
  private long createTime;
  private long lastLoginTime;
  private String avatarUrl;
  private String provinceOfAddress; // 省
  private String cityOfAddress; // 市
  private String regionOfAddress; // 区 
  private String detailedAddress; // 详细地址
  // ...省略其他属性和方法...
}
```
- Some will think we should separate `AddressInfo` from `UserInfo` but it depends
	- If the address only display along the user all the time, current design is suitable
	- If address have different uses (for example use in e-commerce shipment) it is better to have dedicated class

### Rules of Thumb
- When the lines of code, method, attributes become overloaded
- Too much dependency on other class
- Too much `private` method, we should consider separate the `private` method to dedicated class and setup `public` method for reuse
- Hard to name, can only named using general name like `Manager`, `Context`
- Most methods focus on certain subset of attributes

## Do not overdo it
For example, breaking down the below class into `Serializer` and `Deserializer` improve SRP, but damage maintainability. When we need to change `IDENTIFIER_STRING` we need to update 2 places
```java
/**
 * Protocol format: identifier-string;{gson string}
 * For example: UEUEUE;{"a":"A","b":"B"}
 */
public class Serialization {
  private static final String IDENTIFIER_STRING = "UEUEUE;";
  private Gson gson;
  
  public Serialization() {
    this.gson = new Gson();
  }
  
  public String serialize(Map<String, String> object) {
    StringBuilder textBuilder = new StringBuilder();
    textBuilder.append(IDENTIFIER_STRING);
    textBuilder.append(gson.toJson(object));
    return textBuilder.toString();
  }
  
  public Map<String, String> deserialize(String text) {
    if (!text.startsWith(IDENTIFIER_STRING)) {
        return Collections.emptyMap();
    }
    String gsonStr = text.substring(IDENTIFIER_STRING.length());
    return gson.fromJson(gsonStr, Map.class);
  }
}
```


# References

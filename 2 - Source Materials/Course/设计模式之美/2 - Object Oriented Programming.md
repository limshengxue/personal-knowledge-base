2026-04-25 12:54

# 2 - Object Oriented Programming
- Object oriented programming (OOP) is a programming paradigm that build software with the unit of *class* and *object*
- It has 4 main pillars

## Encapsulation
- Implemented using *access modifiers* in OOP language like `private` and `public` in java, must have language supported to be implemented
- Also known as information hiding or information access protection
- `class` expose limited interface for its data to be accessed from external world
- The problem it is trying to solve
	- Prevent unexpected change of attribute of an object
	- Improve *ease-of-use* of the code, the caller doesn't need business knowledge, its behaviors are limited with the encapsulation

## Abstraction
- Hiding implementation details
- Implemented using `interface` and `abstract` 
- Fairly easy to be implemented, doesn't necessary require language support, writing a `function` itself is an abstraction implementation
- Abstraction thinking hides away to implementation, for example `getAwsPictureURL()` have no abstraction but `getPictureURL()` is abstracted
- The problem it is trying to solve
	- Reduce the mental load in programming

## Inheritance
- Represent `is-a` relationship
- Require language support like `extends` in Java
- The problem it is trying to solve
	- Reusability
	- Map to `is-a` relationship naturally in real world
- Over extensive use of inheritance affect code readability and is an anti-pattenr

## Polymorphism
- Child can replace its parent, and we utilise child method's implementation during runtime
```java
public class DynamicArray {
  private static final int DEFAULT_CAPACITY = 10;
  protected int size = 0;
  protected int capacity = DEFAULT_CAPACITY;
  protected Integer[] elements = new Integer[DEFAULT_CAPACITY];
  
  public int size() { return this.size; }
  public Integer get(int index) { return elements[index];}
  //...省略n多方法...
  
  public void add(Integer e) {
    ensureCapacity();
    elements[size++] = e;
  }
  
  protected void ensureCapacity() {
    //...如果数组满了就扩容...代码省略...
  }
}

public class SortedDynamicArray extends DynamicArray {
  @Override
  public void add(Integer e) {
    ensureCapacity();
    int i;
    for (i = size-1; i>=0; --i) { //保证数组中的数据有序
      if (elements[i] > e) {
        elements[i+1] = elements[i];
      } else {
        break;
      }
    }
    elements[i+1] = e;
    ++size;
  }
}

public class Example {
  public static void test(DynamicArray dynamicArray) {
    dynamicArray.add(5);
    dynamicArray.add(1);
    dynamicArray.add(3);
    for (int i = 0; i < dynamicArray.size(); ++i) {
      System.out.println(dynamicArray.get(i));
    }
  }
  
  public static void main(String args[]) {
    DynamicArray dynamicArray = new SortedDynamicArray();
    test(dynamicArray); // 打印结果：1、3、5
  }
}
```
We need 3 language support
- Parent object and hold reference to child reference, the `test` function that accept `DynamicArray` can accepts `SortedDynamicArray`
- Support inheritance and method override

Other than inheritance and method override we can also implement polymorphism with duck-typing
```python
class Logger:
    def record(self):
        print(“I write a log into file.”)
        
class DB:
    def record(self):
        print(“I insert data into db. ”)
        
def test(recorder):
    recorder.record()

def demo():
    logger = Logger()
    db = DB()
    test(logger)
    test(db)
```
Duck-typing only works for dynamic typing language. It means we can use object inter-changeably when both have common methods.
- What problem polymorphism is solving
- Improve extensibility and reusability


# References

2026-04-25 14:32

# 4 - Interface vs Abstract
## Abstract
- Abstract class cannot be instantiated
- Can include method and attribute, and the method can have a body or empty (abstract method)
- When child inherit an abstract class, it must define body for the abstract method
- Still model an `is-a` relationship

### Use Cases
- Code reusability

## Interface
- Interface cannot have attributes
- Interface cannot have method with body
- When class implement interface, it must implement all methods of the interface
- Interface is a *contract*, model `has-a` relationship

### Use Cases
- Decoupling and abstraction

## Program to an interface, not an implementation
- The *interface* here refers to the general idea not the specific `interface` in Java
- It means code that depends on an abstract, high level interface is more flexibility and maintainable

A counterexample is below
```java
public class AliyunImageStore {
  //...省略属性、构造函数等...
  
  public void createBucketIfNotExisting(String bucketName) {
    // ...创建bucket代码逻辑...
    // ...失败会抛出异常..
  }
  
  public String generateAccessToken() {
    // ...根据accesskey/secrectkey等生成access token
  }
  
  public String uploadToAliyun(Image image, String bucketName, String accessToken) {
    //...上传图片到阿里云...
    //...返回图片存储在阿里云上的地址(url）...
  }
  
  public Image downloadFromAliyun(String url, String accessToken) {
    //...从阿里云下载图片...
  }
}

// AliyunImageStore类的使用举例
public class ImageProcessingJob {
  private static final String BUCKET_NAME = "ai_images_bucket";
  //...省略其他无关代码...
  
  public void process() {
    Image image = ...; //处理图片，并封装为Image对象
    AliyunImageStore imageStore = new AliyunImageStore(/*省略参数*/);
    imageStore.createBucketIfNotExisting(BUCKET_NAME);
    String accessToken = imageStore.generateAccessToken();
    imagestore.uploadToAliyun(image, BUCKET_NAME, accessToken);
  }
  
}
```
To adhere to the principle, we need to make 3 changes
- The naming of the method should not exposed its implementation details
- Encapsulate implementation details, for example `generateAccessToken` should not be exposed
- Define `interface` or `abstract`, downstream utilise them

```java
public interface ImageStore {
  String upload(Image image, String bucketName);
  Image download(String url);
}

public class AliyunImageStore implements ImageStore {
  //...省略属性、构造函数等...

  public String upload(Image image, String bucketName) {
    createBucketIfNotExisting(bucketName);
    String accessToken = generateAccessToken();
    //...上传图片到阿里云...
    //...返回图片在阿里云上的地址(url)...
  }

  public Image download(String url) {
    String accessToken = generateAccessToken();
    //...从阿里云下载图片...
  }

  private void createBucketIfNotExisting(String bucketName) {
    // ...创建bucket...
    // ...失败会抛出异常..
  }

  private String generateAccessToken() {
    // ...根据accesskey/secrectkey等生成access token
  }
}

// 上传下载流程改变：私有云不需要支持access token
public class PrivateImageStore implements ImageStore  {
  public String upload(Image image, String bucketName) {
    createBucketIfNotExisting(bucketName);
    //...上传图片到私有云...
    //...返回图片的url...
  }

  public Image download(String url) {
    //...从私有云下载图片...
  }

  private void createBucketIfNotExisting(String bucketName) {
    // ...创建bucket...
    // ...失败会抛出异常..
  }
}

// ImageStore的使用举例
public class ImageProcessingJob {
  private static final String BUCKET_NAME = "ai_images_bucket";
  //...省略其他无关代码...
  
  public void process() {
    Image image = ...;//处理图片，并封装为Image对象
    ImageStore imageStore = new PrivateImageStore(...);
    imagestore.upload(image, BUCKET_NAME);
  }
}
```

### Change our mindset - Abstract, Encapsulate, Interface
- We should think about the Interface before the actual implementation not reverse engineer the Interface based on the implementation
- Overengineer: If we confirm that certain function has only 1 implementation, we can skip defining the Interface
- The more stable the system, the less need to define Interface, vice versa



# References

2026-10-03 13:39

Tags: [[software architecture]]

# Dependency Inversion Principle
- High-level modules and low-level modules should depend on abstractions.
- Abstractions should not depend on implementation details; implementation details should depend on abstractions.
- Keep business policy independent of infrastructure choices such as storage providers, databases, or messaging services.

## Example: Image Storage
- `ImageProcessingJob` represents the application workflow; `AliyunImageStore` provides a particular storage implementation.
- If the job constructs `AliyunImageStore` and manages its access tokens, the workflow depends on provider-specific details.
- Define an `ImageStore` contract around the operations the workflow needs, then make the provider implementation satisfy that contract.
- The source dependencies become `ImageProcessingJob → ImageStore ← AliyunImageStore`.
- At runtime, the job still calls the supplied storage object. DIP changes source-code dependency direction, not the need to execute storage operations.

The following example uses byte arrays for image data:

```java
interface ImageStore {
  String upload(byte[] image, String bucketName);
  byte[] download(String url);
}

class ImageProcessingJob {
  private final ImageStore imageStore;

  ImageProcessingJob(ImageStore imageStore) {
    this.imageStore = imageStore;
  }

  String save(byte[] processedImage) {
    return imageStore.upload(processedImage, "processed-images");
  }
}
```

- Application setup supplies an implementation such as `AliyunImageStore` or `PrivateImageStore`.
- Provider authentication and bucket creation stay inside the implementation.
- Replacing the provider leaves the job unchanged when the replacement honours the same contract.

## Design the Abstraction Around the Consumer
- Describe required capabilities rather than exposing every operation of a provider SDK.
- Keep provider-specific tokens and types out of the contract when the workflow does not need them.
- An interface that merely reproduces provider details can preserve the original coupling despite adding another type.
- The abstraction can be an [[Interfaces vs Abstract Classes|interface or abstract class]], or another suitable contract; DIP is not tied to Java's `interface` keyword.

## DIP vs Dependency Injection
- **DIP** governs which abstractions and details the modules depend on.
- **Dependency injection (DI)** supplies dependency instances from outside the consumer.
- Injecting `AliyunImageStore` directly uses DI, but still binds the consumer to that concrete provider type.
- Injecting an `ImageStore` implementation combines DI with a design that follows DIP, provided the contract remains independent of provider details.

## Benefits and Tradeoffs
- Contain infrastructure changes behind a stable contract.
- Supply alternative implementations or test doubles without changing business logic.
- Keep implementation selection in application setup rather than throughout the workflow.
- Add abstractions where they protect a useful boundary. Extra interfaces and adapters also create maintenance work.

# References
[[2 - Source Materials/Course/设计模式之美/12 - Inversion of Control (IOC) and Dependency Injection (DI)|12 - Inversion of Control (IOC) and Dependency Injection (DI)]]
[[2 - Source Materials/Course/设计模式之美/4 - Interface vs Abstract|4 - Interface vs Abstract]]

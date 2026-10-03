2026-10-03 14:35

Tags: [[software architecture]]

# Law of Demeter
- The Law of Demeter (LoD), also called the principle of least knowledge, encourages a component to know only what it needs about its collaborators.
- Prefer working with immediate collaborators over reaching through them into other objects' internal structures.
- Limiting this knowledge helps contain the effects of implementation changes.

## Avoid Reaching Through Collaborators
- A call such as `order.getCustomer().getAddress().getPostcode()` exposes the caller to several relationships inside the order's object structure.
- If those relationships are implementation details, provide an operation such as `order.shippingPostcode()` that expresses the caller's actual need.
- The order can obtain the information while keeping its internal organisation hidden.
- Assess what knowledge the caller depends on rather than counting dots. Fluent APIs and traversal of deliberately exposed data structures do not automatically indicate a problem.

## Source Example: Document Creation
- The source initially makes `Document` construct a downloader and fetch its own HTML.
- Moving downloading into `DocumentFactory` lets `Document` receive its content without knowing how it was fetched.
- This illustrates the related goal of removing unnecessary dependencies and keeping construction responsibilities clear.

The following version represents HTML content as a string:

```java
interface HtmlDownloader {
  String downloadHtml(String url);
}

class Document {
  private final String url;
  private final String html;

  Document(String url, String html) {
    this.url = url;
    this.html = html;
  }
}

class DocumentFactory {
  private final HtmlDownloader downloader;

  DocumentFactory(HtmlDownloader downloader) {
    this.downloader = downloader;
  }

  Document createDocument(String url) {
    String html = downloader.downloadHtml(url);
    return new Document(url, html);
  }
}
```

- `DocumentFactory` coordinates downloading and creation through its direct collaborator.
- `Document` stores its content without depending on the downloader or network transport.
- Calling a direct collaborator is not itself a LoD violation; evaluate whether the responsibility and dependency belong in that component.

## Limit Exposed Knowledge
- Pass the information a collaborator needs rather than an unrelated object whose internals it must inspect.
- In the source, a general network transporter accepts an address and data instead of depending on an HTML-specific request type.
- Choose cohesive parameters; splitting every object into primitive arguments can also make an interface harder to use.

## LoD vs Interface Segregation
- [[Interface Segregation Principle]] concerns giving a client a contract containing the operations it needs.
- LoD concerns limiting knowledge of collaborators and their internal relationships.
- Separate serializer and deserializer interfaces can narrow client dependencies without splitting a cohesive implementation. This directly applies ISP and supports the broader goal of limited knowledge.

## Tradeoffs
- Add delegating operations when they protect a useful boundary or express domain behaviour.
- Avoid forwarding methods that merely move a dependency around without hiding meaningful details.
- Balance reduced [[Cohesion and Coupling|coupling]] with clear interfaces and understandable execution flow.

# References
[[2 - Source Materials/Course/设计模式之美/15 - LOD for High Cohesive, Loose Coupling|15 - LOD for High Cohesive, Loose Coupling]]

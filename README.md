# barracuda-expression
### Simple expression parser

**Sample Usage:**

```java
    String expression = "" +
            "${encrypt.des(" +
                "request.payload," +
                "${hash.md5(@request.payload, @channel.merchantId2, ${random.long()})}," +
                "@channel.merchantKey," +
                "@channel.merchantKey2" +
            ")}";

    BarracudaCompiler barracudaCompiler = BarracudaCompilerBuilder
            .configure()
            .feature(new EncryptFeature())
            .feature(new HashFeature())
            .feature(new RandomFeature())
            .build();

    BarracudaExpression barracudaExpression = barracudaCompiler.compile(expression);

    Map<String, Object> requestMap = new HashMap<>();
    requestMap.put("payload", "hello");

    Map<String, Object> channelMap = new HashMap<>();
    requestMap.put("merchantKey", "123");
    requestMap.put("merchantKey2", "789");

    String hashValue = barracudaExpression
            .map("request", requestMap)
            .map("channel", channelMap)
            .eval();
```

# barracuda-expression
### Simple expression parser

**Sample Usage:**

```java
    String expression = "" +
            "${encrypt.des(" +
                "myPayload.data," +
                "${hash.md5(@myPayload.data, @myCustomData.key2, ${random.long()})}," +
                "@myCustomData.key," +
                "@myCustomData.key2" +
            ")}";

    BarracudaCompiler barracudaCompiler = BarracudaCompilerBuilder
            .configure()
            .feature(new EncryptFeature())
            .feature(new HashFeature())
            .feature(new RandomFeature())
            .build();

    BarracudaExpression barracudaExpression = barracudaCompiler.compile(expression);

    Map<String, Object> myPayloadMap = new HashMap<>();
    myPayloadMap.put("data", "dGhpcyBpcyBteSBwYXlsb2FkLg==");

    Map<String, Object> myCustomDataMap = new HashMap<>();
    myCustomDataMap.put("key", "12345");
    myCustomDataMap.put("key2", "67890");

    String hashValue = barracudaExpression
            .map("myPayload", myPayloadMap)
            .map("myCustomData", myCustomDataMap)
            .eval(String.class);
```

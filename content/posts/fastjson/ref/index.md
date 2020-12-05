---
title: 解决 fastjson 输出 $ref 引用问题
date: 2020-04-21
tags:
  - fastjson
---

```java
 FastJsonConfig fastJsonConfig = new FastJsonConfig();
 fastJsonConfig.setSerializerFeatures(SerializerFeature.DisableCircularReferenceDetect); // 关闭引用检测
 fastConverter.setFastJsonConfig(fastJsonConfig);
```

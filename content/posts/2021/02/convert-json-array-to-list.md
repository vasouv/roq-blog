---
layout: :theme/post
title: "Convert JSON array to List"
description: How to convert a JSON array to list of objects with Jackson
date: "2021-02-20"
tags: "jackson"
---

With Jackson we can convert a String that contains a JSON array to a List of objects.

```java
new ObjectMapper().readValue(contents, new TypeReference<List<Claymore>>() {});
```

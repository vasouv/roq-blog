---
layout: :theme/post
title: "Read file from the resources directory"
date: "2021-02-20"
tags: 
  - "spring-boot"
---

We want to read a file in the **src/main/resources** directory and return it as a String.

First the ResourceLoader is autowired

```java
@Autowired
ResourceLoader resourceLoader;
```

And then we read the file

```java
Resource resource = resourceLoader.getResource("classpath:claymore.json");
InputStream inputStream = resource.getInputStream();
String contents = new BufferedReader(new InputStreamReader(inputStream)).lines().collect(Collectors.joining("\n"));
```

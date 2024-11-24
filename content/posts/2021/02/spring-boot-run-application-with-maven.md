---
layout: :theme/post
title: "Spring Boot Run Application with Maven"
date: "2021-02-03"
tags: "spring-boot"
---

## Spring Boot 2.x

```text
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```

## Spring Boot 1.x

```text
mvn spring-boot:run -Dspring.profiles.active=dev
```

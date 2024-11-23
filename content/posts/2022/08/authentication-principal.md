---
layout: :theme/post
title: "Authentication Principal"
description: How to get the logged in user through the Security Context in Spring Boot
date: "2022-08-25"
tags: "spring-boot"
---

```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
String currentPrincipalName = authentication.getName();
```

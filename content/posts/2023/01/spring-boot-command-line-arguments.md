---
layout: :theme/post
title: "Spring Boot Command Line Arguments"
description: Passing arguments to a Spring Boot application
date: "2023-01-03"
tags: "spring-boot"
---

In order to pass arguments to a Spring Boot application at runtime, there are a couple of ways to do so.

## Maven

```text
mvn spring-boot:run -Dspring-boot.run.arguments="--msg.public=cmdMsg --server.port=8085"
```

For **Spring Boot 2.3+** the above code will override the **server.port** and **msg.public** property. Command line arguments will always take precedence over the .properties files.

## JVM

```text
java -jar target/secrets-0.0.1-SNAPSHOT.jar --msg.public=jarfile
```

The code above will override the property **msg.public** and set its value to "jarfile".

## IntelliJ

To pass a runtime argument to the configuration, we go to **Edit Configurations** and under **Java Options** select the **Properties** entry. There, simply add the property and value.

![2023/01/intellij-properties.png](https://vasouv.wordpress.com/wp-content/uploads/2023/01/intellij-properties.png?w=1024)

---
layout: :theme/post
title: "WildFly Maven Plugin"
description: Maven commands to deploy a JavaEE application on WildFly
date: "2020-05-02"
categories: "maven"
---

When we're developing Java EE applications, we can use the IDE to deploy the project to the application server. We can however use the Maven plugin to deploy, which can be useful if the IDE doesn't support Java EE or the application server etc. For example, IntelliJ Community Edition doesn't support Java EE or application servers.

In order to enable the plugin:

```xml
<plugin>
    <groupId>org.wildfly.plugins</groupId>
    <artifactId>wildfly-maven-plugin</artifactId>
    <version>2.0.1.Final</version>
    <configuration>
        <jbossHome>~/DEV/wildfly-19.0.0.Final</jbossHome>
    </configuration>
</plugin>
```

To deploy the project for the first time:

```text
mvn wildfly:deploy
```

and to redeploy:

```text
mvn wildfly:redeploy
```

To undeploy run:

```text
mvn wildfly:undeploy
```

[source](https://github.com/hantsy/jakartaee8-starter/blob/master/docs/03run-wildfly-mvn.md)

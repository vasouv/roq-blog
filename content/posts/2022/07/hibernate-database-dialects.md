---
layout: :theme/post
title: "Hibernate Database Dialects"
description: Database dialects for Spring Boot
date: "2022-07-24"
tags: "spring-boot"
---

**Spring Boot Properties**

<figure>
<table><tbody><tr><td>H2</td><td>org.hibernate.dialect.H2Dialect</td></tr><tr><td>MariaDBDialect</td><td>org.hibernate.dialect.MariaDBDialect</td></tr><tr><td>MySQL8Dialect</td><td>org.hibernate.dialect.MySQL8Dialect</td></tr><tr><td>PostgreSQL95Dialect</td><td>org.hibernate.dialect.PostgreSQL95Dialect</td></tr></tbody></table>
</figure>

**Example configuration**

```yaml
spring.datasource.url=jdbc:mariadb://localhost:3306/diet-logs-rev
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MariaDBDialect
spring.jpa.hibernate.ddl-auto=update
```

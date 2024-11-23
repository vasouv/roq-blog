---
layout: :theme/post
title: "Spring Boot Additional Configuration"
description: Adding other configuration files in a Spring Boot application
date: "2023-01-06"
tags: "spring-boot"
---

Spring Boot 2.4 has added the option to have additional configuration properties, other than the usual property files. With this option, it's really simple to add secret properties that we wouldn't want to add to a repository and have them public, such as credentials, database urls etc.

## Set external property

```text
spring.config.import=optional:secrets.properties
```

All we need here is a **secrets.properties** file next to **application.properties**. It will be picked up and add its properties to the context. In this case it's optional, so the application will not fail if the file is not present.

## Gitignore

In case the file indeed contains private information that must not be pushed to a public repository, the file must be included in **.gitignore**.

## Exclude file from JAR

By default, the Spring Boot Maven plugin will include all files in the final package. Therefore it's easy to see the secret properties if we have access to the jar file. In order to exclude it from the package, the following configuration will do it.

```xml
<build>
	<resources>
		<resource>
			<directory>src/main/resources</directory>
			<filtering>true</filtering>
			<excludes>
				<exclude>**/secrets.properties</exclude>
			</excludes>
		</resource>
	</resources>
</build>
```

Of course in this case, the project will crash on runtime if we don't pass the values as arguments. In this [post](https://vasouv.wordpress.com/2023/01/03/spring-boot-command-line-arguments/) I've already written the way to do it for Spring Boot 2+.

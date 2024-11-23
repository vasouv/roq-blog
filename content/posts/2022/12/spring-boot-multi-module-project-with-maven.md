---
layout: :theme/post
title: "Spring Boot Multi Module Project with Maven"
date: "2022-12-14"
tags: 
  - "maven"
  - "spring-boot"
---

A very useful way to organize a project with multiple separate projects, is to use a parent project that handles most of the structure and can build all sub projects, without having to build them separately. Here I'll be showing a way to create a multi module Maven project with Spring Boot.

## Create parent project

In the directory we want to create the project, we have to run the following command to create a generic Maven project that will be used as the parent one.

```text
mvn archetype:generate -DgroupId=vs -DartifactId=multi-module-test -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

## Configure parent project

First thing to do is to delete the **src** directory, clean the pom file from any unwanted configuration and set the **packaging** tag to **pom**. The project version (SNAPSHOT etc) can be set as a **<revision>** property and then referenced as **${ revision }** in the parent pom and all modules.

The parent pom must reference the Spring Boot Parent artifact like so:

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.6</version>
    <relativePath/>
</parent>
```

Each individual module must be set in the **modules** section.

```xml
<modules>
    <module>service1</module>
    <module>service2</module>
</modules>
```

The dependencies used by the modules can be configured in two ways. The **dependencies** section contains all the dependencies that are shared across all modules. The **dependencyManagement** section has the dependencies that some modules need, not all of them will have them.

For example, the following snippet assumes that all modules will have the Spring Boot Starter package, Spring Boot Starter Test package and Lombok, while only some of them might need the Spring Boot Started Web package.

```xml
<!-- All modules will use the dependencies-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

<!-- Some modules will use the dependencies-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

Finally, the Spring Boot Maven Plugin can either be on the parent or individual modules according to the needs of the project.

## Configure module projects

The modules in this example are configured like so; service1 is a web application therefore needs the starter web package, while service2 is a simple Spring Boot application and only uses the mandatory dependencies from the parent. The optional dependencies though (in this case the starter web package) must declare their version in the dependencies section.

### Service 1

```xml
     <parent>
		<groupId>vs</groupId>
		<artifactId>multi-module-tests</artifactId>
		<version>{ revision }</version>
	</parent>

	<artifactId>service1</artifactId>
	<name>service1</name>
	<description>Demo project for Spring Boot</description>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<version>2.7.6</version>
		</dependency>
	</dependencies>
```

### Service 2

```xml
     <parent>
		<groupId>vs</groupId>
		<artifactId>multi-module-tests</artifactId>
		<version>{ revision }</version>
	</parent>

	<groupId>vs</groupId>
	<artifactId>service2</artifactId>
	<name>service2</name>
	<description>Demo project for Spring Boot</description>
```

## Build Project

Finally, when we run the build command in the parent directory we should see all subprojects being built and having the parent version.

---
layout: :theme/post
title: "Spring Boot Security - Basic Authentication"
description: Examples on how to implement Basic Authentication with Spring Security 
date: "2022-10-24"
tags: "spring-boot"
---

In this series of posts I want to document how to use Spring Security to secure an API. This first part will describe a couple of simple use cases using Basic Authentication. Nowadays Basic Authentication is not secure at all and should be avoided, but at the very least it gives us some tools to work with and is good enough for simple projects.

## Project setup

To start with, we need a Spring Boot project with the Web dependency. Then we'll create three controllers like so:

```java
@RestController
class HomeController {

	@GetMapping("/")
	public String home() {
		return "Home Controller";
	}

}

@RestController
class UnsecuredController {

	@GetMapping("unsecured")
	public String unsecured() {
		return "Unsecured Controller";
	}

}

@RestController
class SecuredController {

	@GetMapping("secured")
	public String secured() {
		return "Secured Controller";
	}
}
```

Now if we run the project, we can hit all three controllers and get a response. The end result is to have the Home and Unsecured endpoints available for all users, and only the Secured endpoint accessible to the authenticated user.

## Default security configuration

To enable the default configuration with Spring Security to the project, we need to add the suitable dependency.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

When we run the project we'll notice a couple of things; in the logs there is a line that displays a generated password. This is generated for the default user and can be used as is. Also, all endpoints are secured by default, since the default security configuration is applied on the whole application. So if we try to call any endpoint, we'll get an HTTP 401 response.

So in order to hit any endpoint with the default credentials, we can use Postman to make the call. In the **Authorization** tab, we select **Basic Auth** and then provide **user** for the username, and the generated password.

### Setting custom username and password

We can set our custom credentials in the properties file.

```yaml
spring.security.user.name = vasouv
spring.security.user.password = vasouv
```

No by entering our own credentials in Postman, we can access the endpoints again.

## Override the default configuration

### Override the UserDetailsService with an InMemoryUserDetailsManager

By using the InMemoryUserDetailsManager, we keep the users' credentials in memory instead of a database. This approach is perhaps ideal for a simple project because there's minimal configuration and also we can have multiple users that can authenticate on the application.

#### Before Spring Boot 2.7

```java
@Configuration
public class ProjectConfig extends WebSecurityConfigurerAdapter {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.httpBasic();
        http.authorizeRequests()
            .antMatchers("/", "/unsecured").permitAll()
            .anyRequest().authenticated();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        var userDetailsService = new InMemoryUserDetailsManager();

        var vasouv = User
            .withUsername("vasouv")
            .password(passwordEncoder().encode("vasouv"))
            .authorities("read")
            .build();
        var user123 = User
            .withUsername("user123")
            .password(passwordEncoder().encode("user123"))
            .authorities("read")
            .build();

        userDetailsService.createUser(vasouv);
        userDetailsService.createUser(user123);

        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }

}
```

Here, the password encoder is created as a bean and can be used in the application. The overridden `configure(HttpSecurity http)` method configures HTTP Basic for authentication, leaves the root and `/unsecured` endpoints available to all users, and secures all other endpoints.

The overridden `configure(AuthenticationManagerBuilder auth)` sets up and `InMemoryUserDetailsManager` for the `UserDetailsService`. The users can be added to the UserDetailsService where we can set up the password manager as well. Then, the framework takes over and handles the credentials whenever calls are made to the endpoints.

#### Spring Boot 2.7 and after

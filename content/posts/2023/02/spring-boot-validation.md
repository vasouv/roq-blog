---
layout: :theme/post
title: "Spring Boot Validation"
description: How to validate endpoints and parameters in Spring Boot
date: "2023-02-05"
tags: "spring-boot"
---

By default Spring Boot provides great support to validate the input of controllers and services. In this example I want to demonstrate a couple of ways to validate the data on controllers.

Let's say we have a very basic REST controller to save new music albums, retrieve albums of a specific year and get an album of a specific band. Therefore, we must validate the body of a POST method, path variables and query parameters.

## Rest Controller

### Validate a request body

The POST `/albums` endpoint will receive the following request body

```java
public class Album {

    @NotBlank(message = "Title must not be blank")
    public String title;

    @NotBlank(message = "Band must not be blank")
    public String band;

    @Min(value = 1980, message = "Only years after 1980")
    public int year;
}
```

The constraints here are the title and band fields must not be blank, and the year of the album cannot be less than 1980.

The POST method in the controller is declared like so

```java
@PostMapping
public void save(@Valid @RequestBody Album album) {
    log.info("Saving album: " + album);
}
```

Basically it needs the `@Valid` annotation in the method in order to validate the fields of the class.

The method parameter validation will throw a `MethodArgumentNotValidException` if the validation fails.

## Validate path variables and request parameters

For path variables and request parameters, we have to add the `@Validated` annotation on the class.

```java
@Validated
@RestController
@RequestMapping("albums")
public class AlbumController {}
```

The methods for these validations are like so

```java
@GetMapping("year")
    public Album year(@PathVariable @Min(value = 1980, message = "Not earlier than 1980") int year) {
    return new Album("Some title", "Some band", year);
}
    
@GetMapping
public Album band(@RequestParam("band") @NotBlank(message = "Must provide a band") String band) {
    return new Album("Nightfall in Middle Earth", band, 1998);
}
```

Path variables and request parameters will throw a `ConstraintViolationException` if the validation fails.

## Exception Handling

In order to have a single point that handles all these exceptions we can use a global `ControllerAdvice`, instead of using an `ExceptionHandler` on each controller method.

```java
@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    // for the @Valid annotation in request body
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(
            MethodArgumentNotValidException ex, HttpHeaders headers, HttpStatus status,
            WebRequest request) {

        // Get all errors
        List<String> errors = ex.getBindingResult().getFieldErrors().stream()
                .map(DefaultMessageSourceResolvable::getDefaultMessage).toList();

        return ResponseEntity.status(status).body(new ErrorResponse(status.value(), errors));

    }

    // for the @Validated annotation in path parameters
    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<ErrorResponse> handleConstraintViolation(ConstraintViolationException ex,
                                                                   WebRequest request) {

        List<String> errors = ex.getConstraintViolations().stream()
                .map(v -> String.join(": ", v.getPropertyPath().toString(), v.getMessage()))
                .toList();

        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(new ErrorResponse(HttpStatus.BAD_REQUEST.value(), errors));
    }
}
```

The code and unit tests are available [here](https://github.com/vasouv/validation).

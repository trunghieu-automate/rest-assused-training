# Introduction:
- REST-assured is a Java library that simplifies the testing and validation of REST APIs.
> - This means, you can easily setup it as a library using maven or gradle,... for testing REST API
- It supports the HTTP method like GET, POST, PUT, PATCH, DELETE,...
- REST-Assured is based on the principle of BDD, which means that you write your tests in a way that how that describles how the API should behave.
> - Meaning Rest-assured use given-when-then DSL to write and structure your test scripts. 
- REST-Assured use Hamcrest matchers to express your assertions.

# Features:
- Parsing and validating JSON and XML responses
- Handling cookies, headers, and parameters
- Sending multipart requests and file uploads
- Dealing with different authentication schemes
- Extracting data from responses for further verification
- Reusing specifications and configurations across tests
- Using Groovy’s GPath syntax to query JSON and XML documents
- Validating JSON responses against JSON schemas

## Setting up:
- You can use maven or gradle to set up this dependency.
- With maven: just add dependency to pom.xml
```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>xxx</version>
    <scope>test</scope>
</dependency>
```
> This will allow you to Rest-assured with Java DSL.
- Then, need to add Hmacrest for assertion:
```xml
<dependency>
    <groupId>org.hamcrest</groupId>
    <artifactId>hamcrest-all</artifactId>
    <version>xxx</version>
</dependency>
```

- Here is sample build.gradle for setting up Rest-Assured
```groovy
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'junit:junit:4.13.2'
    // Use rest assured
    testImplementation 'io.rest-assured:rest-assured:4.3.3'
    testImplementation 'io.rest-assured:json-schema-validator:4.3.3'
}

test {
    useJUnitPlatform()
}
```

## Sample using:
- Send GET request and than verify response code.
```java
import static io.restassured.RestAssured.*;
import static io.restassured.matcher.RestAssuredMatchers.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class GetRequestTest {

    @Test
    public void testGetStatusCode() {
        given().
            baseUri("https://jsonplaceholder.typicode.com").
        when().
            get("/posts").
        then().
            statusCode(200);
    }
}
```
- Send GET request and extract some data from the response for verification
```java
import static io.restassured.RestAssured.*;
import static io.restassured.matcher.RestAssuredMatchers.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class GetRequestTest {

    @Test
    public void testGetData() {
        String title = 
        given().
            baseUri("https://jsonplaceholder.typicode.com").
        when().
            get("/posts/1").
        then().
            extract().
            path("title");

        System.out.println("The title of the first post is: " + title);
    }
}
```
- Make a POST request to a URL and send some data in the request body.
```java
import static io.restassured.RestAssured.*;
import static io.restassured.matcher.RestAssuredMatchers.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class PostRequestTest {

    @Test
    public void testPostData() {
        given().
            baseUri("https://jsonplaceholder.typicode.com").
            contentType("application/json").
            body("{\"title\": \"foo\", \"body\": \"bar\", \"userId\": 1}").
        when().
            post("/posts").
        then().
            statusCode(201).
            body("id", equalTo(101));
    }
}
```

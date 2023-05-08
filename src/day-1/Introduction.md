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
#### With maven:
- We just need to add thing to pom.xml
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

#### With Gradle
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

## Sample usage:
#### Make a GET request:
- Send GET request and validate status code and response data
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
#### Make a POST request 
- Make a POST request and validate the response body data
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
#### Validate the header:
- To validate the reponse header, we need to use `header()` method of Response interface.
> `responce.header(param)` method take header type arguments then return its value.
```java
Response response = get("https://demoqa.com/BookStore/v1/Books");
assertEquals("application/json; charset=utf-8", response.header("Content-Type"));
```
- This code will make a GET request to the given URL and assert that the Content-Type header is equal to “application/json; charset=utf-8”. If the assertion fails, it will throw an AssertionError
- We can use `headers()` method to get all headers of the responce object
```java
Response response = get("https://demoqa.com/BookStore/v1/Books");
Headers headers = response.headers();
for (Header header : headers) {
    System.out.println(header.getName() + ": " + header.getValue());
    // add any validation logic here
}
```
- Validate header value matching with a RE, using matchPattern() method in Hamcrest API
```java
get("https://demoqa.com/BookStore/v1/Books").then()
    .assertThat()
    .header("Content-Type", matchesPattern("application/.*"));
```
- Validate header value contains a substring, using containsString() (Hamcrest API)
```java

get("https://demoqa.com/BookStore/v1/Books").then()
    .assertThat()
    .header("Content-Type", containsString("json"));
```
- Validate that the response has a specific header with a value satisfying a Hamcrest matcher:
```java
get("https://demoqa.com/BookStore/v1/Books").then()
    .assertThat()
    .header("Content-Type", equalToIgnoringCase("application/json; charset=utf-8"));
```

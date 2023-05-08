In RA, we can send many types of params in a request by using methods in [`RequestSpecification`](https://www.javadoc.io/doc/io.rest-assured/rest-assured/latest/index.html) interface.

## queryParam() method:
- Thid method help you to send specific request with with the query parameter.
> A query parameter is a key-value pair that is appended to the URL after a question mark (?).
- - Example:
```java
given()
  .queryParam("q", "java")
.when()
  .get("https://demoqa.com/BookStore/v1/Books");
```
- To specify multiple query parameters, you can either chain multiple queryParam() methods or use the queryParams() method with a map of key-value pairs.

## param() method:
- You can also use the `param()` method for form parameters, this will specify a multi-value parameter that'll be sent with the request
```java
given()
  .param("cars", asList("Honda", "BMW"))...;
```
> This will set the parameter cars=Honda and cars=BMW.

## pathParam():
- This method allows you to specify a path parameter for the request.
>  A path parameter is a variable that is part of the URL path
- - Example: if you want to send a GET request to https://demoqa.com/BookStore/v1/Books/9781449325862 where 9781449325862 is the ISBN of a book, you can write something like this:
```java
given()
  .pathParam("ISBN", "9781449325862")
.when()
  .get("https://demoqa.com/BookStore/v1/Books/{ISBN}");
```
- To specify multiple path parameters, you can either chain multiple `pathParam()` methods or use the `pathParams()` method with a map of key-value pairs.

## formParam():
- This method allows you to specify a form parameter for the request.
> A form parameter is a key-value pair that is sent in the body of a POST request. 
- For example, if you want to send a POST request to https://demoqa.com/Account/v1/User with username and password as form parameters, you can write something like this:
```java
given()
  .formParam("username", "john")
  .formParam("password", "1234")
.when()
  .post("https://demoqa.com/Account/v1/User");
```
- To specify multiple form parameters, you can either chain multiple formParam() methods or use the formParams() method with a map of key-value pairs.

## header(): 
- This method allows you to specify a header for the request. 
> A header is a key-value pair that is sent in the request header. 
- For example, if you want to send a GET request to https://demoqa.com/BookStore/v1/Books with Accept and Content-Type headers, you can write something like this:
```java
given()
  .header("Accept", "application/json")
  .header("Content-Type", "application/json")
.when()
  .get("https://demoqa.com/BookStore/v1/Books");
```
- To specify multiple headers, you can either chain multiple header() methods or use the headers() method with a map of key-value pairs.

## Example for combination of these methods:
- Send a GET request with query and path parameters:
```java
given()
  .queryParam("q", "fiction")
  .pathParam("ISBN", "9781449325862")
.when()
  .get("https://demoqa.com/BookStore/v1/Books/{ISBN}");
```
- Send a POST request with form and header parameters:
```java
given()
  .formParam("username", "john")
  .formParam("password", "1234")
  .header("Authorization", "Basic am9objoxMjM0")
.when()
  .post("https://demoqa.com/Account/v1/User");
```
- Send a PUT request with query, path and header parameters:
```java
given()
  .queryParam("token", "abc123")
  .pathParam("userId", "1234")
  .header("Content-Type", "application/json")
  .body("{\"name\":\"John\",\"email\":\"john@example.com\"}")
.when()
  .put("https://demoqa.com/Account/v1/User/{userId}");
```

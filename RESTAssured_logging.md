# REST Assured - logging

If you would like to use rest assured for long term projects, you may come up with huge amount of tests. You should manage all tests somehow and common topics should be manageable.
Logging is one of the things which should be configurable.
There is one of the solutions how to do that.

Rest assured allows users to log request (like parameters, request body etc.) and response (like status code, response body, cookies etc.)
Thats why better to use different configuration variable for requests and responses. But also possible to have simple configuration to have or not have logging using only one variable. 

Lets create simple test with one request.
```
    @Test
    public void oneTest() {
        given()
                .spec(logging())
                .expect().statusCode(HttpStatus.SC_OK)
                .when()
                .get( "https://www.google.com/" )
                .then()
                .extract()
                .response();
    }
```
There is spec() function, which allows to add specification to your request. This step calls logging() method. Example of it you can find below:
```
    private RequestSpecification logging(){
        RequestSpecification requestSpecification;

        switch (request_loggingMode) {
            case "all":
                requestSpecification = given().filter(new RequestLoggingFilter()).filter(new ResponseLoggingFilter());
                break;
            case "parameters":
                requestSpecification = given().filter(new RequestLoggingFilter(PARAMS));
                break;
            case "no logging":
                requestSpecification = given();
                break;
            default: requestSpecification = given();
        }

        switch (response_loggingMode) {
            case "all":
                requestSpecification.filter(new ResponseLoggingFilter());
                break;
            case "cookie":
                requestSpecification.filter(new ResponseLoggingFilter(LogDetail.COOKIES));
                break;
            case "no logging":
                requestSpecification = given();
                break;
            case "badRequests only":
                requestSpecification = given().filter(new ResponseLoggingFilter(HttpStatus.SC_BAD_REQUEST)).
                        filter(new ResponseLoggingFilter(HttpStatus.SC_EXPECTATION_FAILED)).
                        filter(new ResponseLoggingFilter(HttpStatus.SC_FAILED_DEPENDENCY));
                break;
            default: requestSpecification = given();
        }
        return requestSpecification;
    }
```
Above method use variables for configuration logging type. Don't forget to define them and put one of the values from this method.
Filter method can be added few times and we can log only bad requests.

This idea can improve your logging for CI pipelines to reduce logging of unnecessary information. 







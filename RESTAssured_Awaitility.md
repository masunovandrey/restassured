# Rest assured - awaitility? wait...

API is not always stable enough. Especially for new applications, when you don't have proper environment. Also possible that its just design issue or external API which you need to use.
What to do if you would like to test API, but from time to time you got connection errors? One of the solutions is awaitility library!

Example of using:

step in the test will be look like this:
```
await().pollDelay(delay, TimeUnit.SECONDS).atMost( secondsTimeout, TimeUnit.SECONDS ).until(executePost_callable(body, HttpStatus.SC_OK, BASEAPI + "/entries"));
```
main difference which awaitility need, it callable method. Below you can find example of callable method.
```
  public static Callable<Boolean> executePost_callable(final Object body, final int statusCode, final String method ) {
    return () -> {
      boolean result;
      Response response = given()
              .spec( REQUESTSPECIFICATION )
              .body( body )
              .contentType( ContentType.JSON )
              .accept( ContentType.JSON )
              .when()
              .post( method )
              .then()
              .extract()
              .response();
      int actualStatus = response.getStatusCode();
      result = statusCode == actualStatus;
      return result;
    };
  }
```
Before assertion for status was inside of the request. If you leave it there you got exception soon (our API is not stable, do you remember?)
For callable method you need to do all assertion after request and return assertion result as return value.

Normal workflow for above step: 
1. we make a post request once
2. check if return status matches with our expectations
3. if not, callable method executePost_callable return false
4. awaitility wait delay seconds and repeate post request again until request wil be successfull or awaitility exceed secondsTimeout seconds.

Above example allow you to get data from unstable API and you can create more stable API tests.

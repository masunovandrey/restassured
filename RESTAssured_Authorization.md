# REST Assured - Authorization

If you don't have any experience in API testing, you can start with any API which you can find [here](https://any-api.com/).
There are a lot of open API which mostly have GET endpoints.

Other API usually need authorization.
Let's take spotify API.

There is GET /me endpoint, which return user information. Attempt of execution will show you, that spotify need your authorization. Do it with facebook account or create new account for spotify. Then you can execute request.
In devtool you can find information about request, response, additional parameters etc.

We are making request from any-api.com to spotify, thats why Request URL looks like this.

Lets take url for spotify and insert into our previous request.
```
@Test    
    public void oneTest() {
        given().log().all()
                .expect()              
                .when()
                .get("https://api.spotify.com/v1/me")
                .then()
                .log().all()
                .extract()
                .response();    
    }
```    

Response will be:

```
{
    "error": {
        "status": 401,
        "message": "No token provided"
    }
}
```

Oh no, we forget about authorization.
Refresh any-api page, open DevTools and make manual authorization before executing GET request.
Find access_token request and look at Response tab. This is token which we need. Copy and paste it to our updated request

```
    @Test    
    public void oneTest() {
        given().log().all()
                .auth().oauth2("BQAjOnDNS4WPURvjlIKo9Gsp................zVST4Ppj9XfA")
                .expect()               
                .when()
                .get("https://api.spotify.com/v1/me")
                .then()
                .log().all()
                .extract().response();    
    }
```
Response should include information about user like display name, id, account information etc.
This access token will expire in some time. So, if its real project, we need to know exact steps for token generation and they should be executed automatically. Developers should provide you this information.

Lets modify our test and add assertion for response.
Add response variable and add .extract().response() at the end of request. This allows you to store response and use it for further actions.
Also add simple assertion. asString() convert response to string and we can easily check if it contains user name for example.   

```
@Test    
    public void oneTest() {
        Response response = given().log().all()
                .auth().oauth2("BQAjOnDNS4WPURvjlIKo9Gsp................zVSTIM4j9XfA")
                .expect()
                .statusCode(HttpStatus.SC_OK)
                .when()
                .get("https://api.spotify.com/v1/me")
                .then()
                .log().all()
                .extract().response();        
        Assert.assertTrue(response.asString().contains("Ivan Ivanov"));    
    }
```
Above example shows simplest case of request with authorization and assertion of response.

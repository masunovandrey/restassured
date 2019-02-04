# RESTAssured - test example

#### If you want to start writing tests using rest assured, you need to install:
- IDE ([Eclipse](https://www.eclipse.org/downloads/packages/), [Itellij IDEA](https://www.jetbrains.com/idea/)) 
- Project management tool ([Maven](https://maven.apache.org/), [Gradle](https://gradle.org/))
- [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (don't forget about setup of environment variables)

Assume that you already know how to create maven or gradle projects. 
Create one of them.

Open pom.xml for Maven project or build.gradle for Gradle project and add dependency for rest assured library.

Maven:
```

<!-- https://mvnrepository.com/artifact/com.jayway.restassured/rest-assured -->
<dependency>
    <groupId>com.jayway.restassured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>2.9.0</version>
    <scope>test</scope>
</dependency>
```

Gradle:

```
// https://mvnrepository.com/artifact/com.jayway.restassured/rest-assured
testCompile group: 'com.jayway.restassured', name: 'rest-assured', version: '2.9.0'
```

Then you can start using rest assured. Create test class in the src/test like this:

```
    @Test    
    public void oneTest() {
        given().log().all()
                .expect()
                .statusCode(HttpStatus.SC_OK )
                .when()
                .get( "https://www.google.com/" )
                .then()
                .log().all();
    }
```

Above test is making only one get request to google main page and expect 200 status code in response. Checking status code is simplest quality gate for requests. 

Also there are .log().all() before and after get request allow to see full request and response in the console, which is helpful for analyses, but for CI/CD pipelines better reduce logging to reduce memory usage.

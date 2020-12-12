# Getting Started

This repository illustrates the use of springfox with spring MVC, with Maven. Dependency addition can be done using gradle as well.
Springfox is a library used for JSON API specification and documentation such as swagger, RAML and jsonapi. This repository shows the example of swagger.

### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/2.4.0/maven-plugin/reference/html/)
* [Spring Web](https://docs.spring.io/spring-boot/docs/2.4.0/reference/htmlsingle/#boot-features-developing-web-applications)
* [Springfox](https://springfox.github.io/springfox/docs/current/#introduction)

This application shows a basic spring and spring fox integration apps and points out critical steps for integration.

#### Springfox
Springfox library enables developers to enable documentation of APIs easily. This library was initially called as swagger-springmvc.
Steps to integrate the Springfox as follows, with the description as what each step is meant for.

1. Add dependency
    * Springfox from version 3.0 onwards needs only single dependency as <b>springfox-boot-starter</b>.
    ```xml
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-boot-starter</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```
    Prior to version 3.0.0 needs two dependencies.
   ```xml
   <dependencies>
    <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>2.9.2</version>
    </dependency>
    <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.9.2</version>
    </dependency>
   </dependencies>
    ```
2. Enable Springfox swagger (required)
    * To enable springfox swagger need configuration or application class of project should be labelled with @EnableSwagger2 annotation.
      ```java
       import org.springframework.context.annotation.Configuration;
       import springfox.documentation.swagger2.annotations.EnableSwagger2;
      
       @Configuration
       @EnableSwagger2
       public class SwaggerConfig {
      
      }
      ```

3. Instruct spring to scan APIs for specification.
    * Using component scan base package and class scanning.
      ```java
       import org.springframework.context.annotation.ComponentScan;
       import org.springframework.context.annotation.Configuration;
       import springfox.documentation.swagger2.annotations.EnableSwagger2;
      
       @Configuration
       @EnableSwagger2
       @ComponentScan(basePackages = "com.spring.swagger.controller")
       public class SwaggerConfig {
      
      }
        ```
   
    * Using Docket
        - Docket is a primary API configuration mechanism for springfox.
        - Docket API is initialized with documentation type for SWAGGER_2 it has
          to be initialized with DocumentationType.SWAGGER_2.
        - select() returns an instance of ApiSelectorBuilder to give fine grained control over the endpoints exposed
          via swagger.
        - paths() allows selection of Path's using a predicate. The example here uses an any predicate (default). Out of the box we provide predicates for regex, ant, any, none.
        
        Example:
        ```java
        import org.springframework.context.annotation.Configuration;
        import springfox.documentation.swagger2.annotations.EnableSwagger2;
        
        @Configuration
        @EnableSwagger2
        public class SwaggerConfig {
            @Bean
            public Docket postsApi() {
                return new Docket(DocumentationType.SWAGGER_2)
                    .select().paths(postPaths()).build();
             }
        }
        ```

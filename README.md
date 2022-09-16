### Spring Security - Add Spring Security Maven Dependencies

<img src="https://user-images.githubusercontent.com/80107049/190683955-528abe28-383b-451c-a678-5af3ef801d52.png" width="500" />



**Spring Projects**

<img src="https://user-images.githubusercontent.com/80107049/190684022-fe11be04-cbdc-4e95-85df-c5a15feb1d36.png" width= "500" />


+ These are two **separate** projects
+ The project are on **different** release cycles
+ Version numbers between projects are generally **not in sync**
  + Spring team is working to resolve this for future releases ...

+ Common pitfall is using incompatible projects
+ Need to find compatible version

**Finding Compatible version**

+ Find desired version of Spring Security in Maven Central Repo
  + spring-security-web

+ Look at the project POM file
  + Find supporting Spring Framework version


**Add Maven dependencies for Spring Security**

File:pom.xml
```XML
     <properties>
        <springframework.version>5.3.22</springframework.version>
        <springsecurity.version>5.7.3</springsecurity.version>
    </properties>

  ...
        <!--Spring-->
        <dependencies>
        <!-- Spring MVC support -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${springframework.version}</version>
        </dependency>

        <!--Spring Security-->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${springsecurity.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${springsecurity.version}</version>
        </dependency>
          
```


### Spring Security - Basic Security (Users, Passwords and Roles)

**Development Process**

1. Create Spring Security Initializer
2. Create Spring Security Configuration (@Configuration)
3. Add users, password and roles

**Spring Security Web App Initializer**

+ Spring Security provides support for security initialization
+ Security code is used to initialize the servlet container
+ Special class to register the **Spring Security Filters**

```JAVA
        AbstractSecurityWebApplicationInitializer
```
+ Special class to register the **Spring Security Filters**

<img src="https://user-images.githubusercontent.com/80107049/190684166-11005a70-699e-4f19-aff5-7496176992d0.png" width="500" />


+ TO DO list
  + Extend this abstract base class


_Step 1:Create Spring Security Initializer_

File:SecurityWebApplicationInitializer.java
```JAVA
import org.springframework.security.web.context.AbstractSecurityWebApplicationInitializer;

public class SercurityWebApllicationInitializer extends AbstractSecurityWebApplicationInitializer {
}
```

_Step 2:Create Spring Security Configuration_

File:DemoSecurityConfig.java
```JAVA

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class DemnoSecurityConfig extends WebSecurityConfigurerAdapter {
  
}
```

_Step 3:Add users,passwords and roles_

File:DemoSecurityConfig.java
```JAVA
@Configuration
@EnableWebSecurity
public class DemoSecurityConfig extends WebSecurityConfigurerAdapter {
  
  @Override
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    
    // add our user for in memory authentication
    
    UserBuider users = User.withDefaultPasswordEncoder();
    
    auth.inMemoryAuthentication()
      .withUser(users.username("john").password("test123").roles("EMPLOYEE"))
      .withUser(users.username("mary").password("test123").roles("MANAGER"))
      .withUser(users.username("susan").password("test123").roles("ADMIN"));
    
  }
}
```
+ `UserBuider users = User.withDefaultPasswordEncoder();` default passwords use plain text
      
   
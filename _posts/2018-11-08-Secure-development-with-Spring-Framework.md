---
layout: post
title: Secure development with Spring Framework 
author: jakabakos
author_name: "Ákos Jakab"
author_web: ""
featured-img: spring_security
---
 In the past decade, Spring Framework became a well established and prominent web framework for developing Java applications. The most exciting and essential changes in the Spring ecosystem was the birth and progression of Spring Boot. No matter what you need, the Spring Boot provides comprehensive, easy-to-use and interdisciplinary development environment, tools for deployment and assists in the whole development lifecycle.

<!--excerpt-->

----
One of the major strong points of the Spring Boot is its auto-configuration capability that attempts to automatically configure the Spring application based on the .jar dependencies we have added. This key feature related to the Spring Boot starters gives us freedom, guarantees quick development time and saves us from the burden of specific metadata configuration.  However, applying security to our application is a perfect example when this auto-configuration capability is not enough as the out of the box spring security configuration usually requires fine-tuning. The Spring Security starter is a one-size-fits-all solution, and there are many details to be specified here. First, we have to override the basic auto-configuration and define our custom-tailored security requirements based on the available classes on the classpath.

Spring Security is a powerful and highly customizable authentication and access-control framework. In the following, we will show the coolest Spring Security features and best practices.

## The magical WebSecurityConfigurerAdapter

By implementing a security configuration class, we can, for example, override the default security settings, specify permissions, define users, roles and a custom login page view. For this, we have to extend the WebSecurityConfigurerAdapter. By default, the following configuration is set up in the WebSecurityConfigurerAdapter class. This grants authenticated users access all URLs. 

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
        .antMatchers("/users/**")
            .hasRole("USER")//USER role can access /users/**       
        .antMatchers("/admin/**")
            .("hasRole('ROLE_ADMIN')") //ADMIN role can access /admin/**
        .and()
        .formLogin() //enabling formlogin
            .loginPage("/login") //deafult login page URL
            .failureUrl("/login?error") //deafult login error URL
            .usernameParameter("username") // default parameter name of username
            .passwordParameter("password") // default parameter name of password
        .and()
        .logout()
            .logoutSuccessUrl("/login?logout") // redirect URL after logging out
  }
}
```

## Using HTTPS 

To use HTTPS, we need an SSL certificate. With Spring Boot we can enable HTTPS with a generated self-signed certificate for testing purposes. In the development phase, there is no easier way to get a certificate. However, to go public, we need publicly signed certificates to identify the service providers authenticity. So it’s  not recommended to use self-signed certificate in production. To go public, you will need an SSL (Secure Sockets Layer) certificate from a Certificate Authority. There are some free certificate providers but usually, their support isn’t satisfying. The most reliable and free certificate provider is Let's Encrypt. It is high quality, widely known and used.

In the long run it won’t be sufficient but to get started in development, you can generate a self-signed certificate with a certificate management utility called keytool. Use this command:

```
keytool -genkeypair -alias tomcat -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650
```

This will generate a PKCS12 keystore with a generated certificate, with certificate alias tomcat. Put your generated keystore.p12 file into your project’s resources directory and add these lines to your application.properties:

```
server.port = 8888
# Automatically blocking requests coming from http
security.require-ssl = true
server.ssl.key-store = classpath:keystore.p12
server.ssl.key-alias = tomcat
server.ssl.key-store-password = yourpassword
server.ssl.key-store-type = PKCS12
```
If you already have your SSL certificate, you can import it to a keystore to enable HTTPS in your Spring Boot app. 

**With this command, you can also create a new keystore containing your certificate:**

```
keytool -import -alias tomcat -file myCertificate.crt -keystore keystore.p12 -storepass password
```
## The awesome built-in CSRF protection 

You might be familiar with OWASP’s definition of CSRF: „[Cross-Site Request Forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker's choosing.” 

As [Adam Barth et al. wrote](https://seclab.stanford.edu/websec/csrf/csrf.pdf), there are three widely used techniques for defending from CSRF attacks:

* *validating a secret request token:* the most popular CSRF defense is including a secret token with each request and validating the received token is correctly bound to the user’s session. This prevents CSRF by forcing the attacker to guess the token of the session. 

* *validating the HTTP Referer header* is the simplest CSRF defense that prevents CSRF by accepting requests only from a trusted source

* *validating custom headers attached to XMLHttpRequests:* sites can be defended from CSRF with a custom header set via XMLHttpRequest. Make sure to check the header is present before processing state-modifying requests.

As [Adam Barth et al.](https://seclab.stanford.edu/websec/csrf/csrf.pdf) conclude in their article, there are three widely used techniques for defending from CSRF attacks but none of these techniques are satisfactory on their own.

Spring Boot can prevent a CSRF attack. The CSRF protection is active by default, thus we do not have to configure it explicitly. If you use *Thymeleaf*, the CSRF token will automatically be added as a hidden input field. With using JSP you have to add it to the form by yourself:

```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/> 
```
You can also disable CSRF protection, however, it is not recommended. 

It is recommended to use `POST` requests instead of `GET` to pass CSRF tokens, as by default Spring does not protect `GET` methods. This is a general requirement for proper CSRF prevention. For more details, visit  the [CSRF Spring Documentation](https://docs.spring.io/spring-security/site/docs/3.2.0.CI-SNAPSHOT/reference/html/csrf.html).

**There are different CSRF protection types that Spring offers:**

### Stateful CSRF protection

In this case, the backend generates a token and sends it to the user's browser that has to be copied to an HTTP header before a request to an API endpoint is made.
The server has to compare the token appearing in the header with the one it sent to the browser. If the tokens are equal, then it is accepted. This works because an evil website is not able to read the browser's cookie that contains the CSRF-protection token and to copy it to the HTTP header. 

### Stateless CSRF protection

If maintaining the state for CSRF token at server side is problematic, it is still possible to check that the tokens are the same in order to make sure the API request is not a CSRF attack. The client’s browser generates a (cryptographically strong) pseudorandom value and sets it as a cookie on the user's side, separately from the session identifier. This value is sent with the request to the backend API as a hidden form value or another request parameter or header. If the values are not equal, the server will reject the request. This way of protection can be implemented by creating a new filter.

### Samesite cookie attribute

Definition by OWASP: "SameSite prevents the browser from sending the cookie along with cross-site requests. The main goal is mitigating the risk of cross-origin information leakage. It also provides some protection against cross-site request forgery attacks."

Possible values for the flag are:

* Lax: with this value defined the server maintains the user's logged-in session after the user arrives from an external link

* Strict: it will prevent the browser from sending the cookie to the target site in all cross-site browsing context

**In Spring Security you can easily do this with a filter, like this:**

```java
public class SameSiteFilter extends GenericFilterBean {
    @Override
    public void doFilter (ServletRequest request,  ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse resp = (HttpServletResponse) response;
        resp.setHeader("Set-Cookie", "HttpOnly; SameSite=strict");
        chain.doFilter(request, response);
    }
}
```

Then add this filter to your SecurityConfig class:

```java
http.addFilterAfter(new SameSiteFilter(), BasicAuthenticationFilter.class)
```

Although Spring Security provides a comprehensive toolkit to prevent the most common types of CSRF attacks, it can also be tricked with subdomain takeover and man in the middle attack because the tokens are not tied to the user identity and are not signed. To mitigate this do not expose an HTTP endpoint, only HTTPS and do not allow external content on subdomains.

On Avatao platform you can get a deeper understanding of the [CSRF protection](https://platform.avatao.com/paths/d667233e-ed36-40cc-9491-8aa29e25fdc1/challenges/f16b087e-7520-4204-a135-958da1235005). 

## Method-level Authorization 

Sometimes we need to secure methods with Java configuration. Using `@EnableGlobalMethodSecurity` annotation we can ensure the security of the service layer. By defining roles, we can assure that users can only execute or trigger specific methods. 

For method level authorization, first, we need: 

```java
@Configuration
@EnableGlobalMethodSecurity(
  prePostEnabled = true, 
  securedEnabled = true, 
  jsr250Enabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
	//...
}
```

`prePostEnabled`  is for enabling Spring Security pre and post annotations.  With `securedEnabled` , we can enable the `@Secured`  annotation. The `jsr250Enabled`  property allows us to use the `@RoleAllowed annotation`.

```java
public interface FlightService {
	List<Flight> findAll();
    
	@Secured("ROLE_ADMIN")
	void updateFlight(Flight flight);
    
	@Secured({ "ROLE_USER", "ROLE_ADMIN" })
	void makeReservationForFlight(Flight flight);
}
```

We can define a list of security configuration attributes for service methods with `@Secured` annotation. If anyone tries to invoke a method and does not possess the required roles/permissions, an `AccessDenied` exception will be thrown. In the above example, anyone with an `ADMIN` role can invoke the `updateFlight` method.  For `makeReservationForFlight` method both `USER` or `ADMIN` roles are appropriate.

## Encoding sensitive data

Storing passwords as plain text is perhaps one of the worst security practices and by default forbidden by  Spring Security. There are a few encoding mechanisms Spring Security supports,  like SHA256, SHA512, PBKDF2, BCrypt, and SCrypt. In our example, we will use BCrypt, one of the best available solutions. The `PasswordEncoder` interface has only two methods: `encodePassword` and `isPasswordValid`.

You can encode the password during authentication by defining a `PasswordEncoder` bean in the security configuration class:

```java
public class SecurityConfig extends WebSecurityConfigurerAdapter{
//...
@Bean
   public PasswordEncoder encoder() {
       return new BCryptPasswordEncoder(11);
   }
//...
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
      auth.userDetailsService(
	auth.getDefaultUserDetailsService()).passwordEncoder(encoder());
   }
}
```

The Spring provides a simple solution to encode the password in the database. Let’s see how to store a user with encoded password:

```java
import org.springframework.security.crypto.password.PasswordEncoder;
//...
@Autowired PasswordEncoder passwordEncoder;
//...	
User john = new User("john", passwordEncoder.encode("johnny1"));
List<User> users = new ArrayList<>();
users.add(john);
userRepository.save(users);
```

## Avoiding XSS Attacks

When an XSS (Cross-Site Scripting) attack is performed, a malicious script is injected into a website usually by using an input field to send the malicious code. There are a wide range of XSS attack vectors, however, we can avoid most of these by using choosing proper web frameworks, input validators/filters/encoders and right configurations such as Content Security Policy (CSP).
Spring Security allows users to efficiently inject the default security headers. 

Current Default Security Headers provided by Spring Security:
* Cache-Control
* Content-Type Options
* HTTP Strict Transport Security
* X-Frame-Options
* X-XSS-Protection

With using Spring configuration, all default security headers are set. 

## Avoiding clickjacking

Clickjacking is a technique of tricking a web-user into clicking on something different from what the user perceives.  This - usually malicious - click is triggering an action in a hidden or transparent iframe. For example, a logged in user in a bank’s system might click on a button that gives access to someone else. Allowing your website to be shown in an iframe can create a new attack vector for clickjacking.

There are some ways to avoid clickjacking actions. You can use frame breaking code, however, a more modern approach is suggested to avoid clickjacking with the X-Frame-Options header set to `DENY`. Luckily, Spring Security automatically sets the X-Frame-Options to `DENY`, thus whenever you need to change it, you will need to do so it explicitly.

We can also selectively enable the frame options with Java annotations, adding the required header

```java
@EnableWebSecurity
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.headers().frameOptions();
  }
}
```

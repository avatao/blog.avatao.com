---
layout: post
title: Secure development with Spring Framework 
author: jakabakos
author_name: "Ákos Jakab"
author_web: ""
featured-img: 
---

 In the past decade, Spring Framework became the de facto standard for developing Java applications. The most exciting and essential changes in the Spring ecosystem – and in the Java development – was the birth and progression of Spring Boot. No matter what you need, the Spring Boot provides comprehensive, easy-to-use and interdisciplinary development environment, tools for deployment and assists in the whole development lifecycle.

<!--excerpt-->

----

One of the major strong points of the Spring Boot is its auto-configuration capability that attempts to automatically configure the Spring application based on the .jar dependencies we have added. This key feature related to the Spring Boot starters gives us freedom, guarantees quick development time and saves us from the burden of specific metadata configuration.   Applying security to our application is a perfect example when this auto-configuration capability is not enough. The Spring Security starter is a one-size-fits-all solution, and there are many details to be specified here. First, we have to override the basic auto-configuration and define our custom-tailored security requirements.

Spring Security is a powerful and highly customizable authentication and access-control framework. In the following, we will show the coolest Spring Security features and best practices.

## The magical WebSecurityConfigurerAdapter

By implementing a security configuration class, we can, for example, override the default security settings, specify permissions, define users, roles and a custom login page view. For this, we have to extend the WebSecurityConfigurerAdapter. For example:

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
    .antMatchers("/admin/**").access("hasRole('ROLE_ADMIN')")
    .and()
    .formLogin().loginPage("/login").failureUrl("/login?error")
    .usernameParameter("username").passwordParameter("password")		
    .and()
    .logout().logoutSuccessUrl("/login?logout")
  }
}
```
## Using HTTPS 

To use HTTPS, we need an SSL certificate. With Spring Boot we can enable HTTPS with a generated self-signed certificate for testing purposes. In the development phase there is no easier way to get a certificate, but because of a warning sign, untrusted certificate, it’s  not recommended to use it in production. To go public, you will need  an SSL (Secure Sockets Layer) certificate from a Certificate Authority. There are some free certificate providers but usually their support isn’t satisfying. 

On the long run it won’t be sufficient but to get started in development, you can generate a self-signed certificate with a certificate management utility called keytool. Use this command:

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

### With this command, you can also create a new keystore containing your certificate:

```
keytool -import -alias tomcat -file myCertificate.crt -keystore keystore.p12 -storepass password
```

## The awesome built-in CSRF protection 

You are might familiar with the OWASP’s definition of CSRF: „[Cross-Site Request Forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker's choosing.” 

Spring Boot can prevent a CSRF attack, the CSRF protection is active by default. If you use Thymeleaf, the CSRF token will automatically be added as a hidden input field. With using JSP you have to add it to the form by yourself:

```
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/> 
```

You can also disable it in the class by adding this:

```
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter{

@Override
   protected void configure(HttpSecurity http) throws Exception {
      http.csrf().disable();
   }
}
```
For sensitive information, it is recommended to use POST instead of GET. This is a general requirement for proper CSRF prevention to avoid sensitive information leaking. For more details, visit the [CSRF Spring Documentation](https://docs.spring.io/spring-security/site/docs/3.2.0.CI-SNAPSHOT/reference/html/csrf.html).

On Avatao platform you can get a deeper understanding about the [CSRF protection](https://platform.avatao.com/paths/d667233e-ed36-40cc-9491-8aa29e25fdc1/challenges/f16b087e-7520-4204-a135-958da1235005). 


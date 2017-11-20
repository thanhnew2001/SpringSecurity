HelloHibernateSpringMVCSecurity

Now we need to extend our SpringMVC so that it can protect our resources from unauthorized access. We use SpringSecurity for this purpose. In most cases we use a database to persist our user details (username, password, and authorities) but for testing purpose we can use InMemory user.

Often, after users login successfully, there is a session created on server and send back to the client and be stored as a cookie. Subsequent requests from that client will be accompanioned by that cookie, thus they are allowed to access a resource.

For restful webservices, most of requests are stateless, meaning that it is not aware of any no pre-authentication. The approach in such a case is to send again username/password in the header (as in BasicAuthentication protocol). There is a better way, however, often used in token based request, is to send in the header a token which contains the status of the resquest (user details, authorization, time to expires etc).

This example shows how we can use in memory users to test Spring Security

  WebSecurityConfigurerAdapter

The @EnableWebSecurity annotation and WebSecurityConfigurerAdapter work together to provide web based security. By extending WebSecurityConfigurerAdapter and only a few lines of code we are able to do the following:

Require the user to be authenticated prior to accessing any URL within our application Create a user with the username “user”, password “password”, and role of “ROLE_USER” Enables HTTP Basic and Form based authentication Spring Security will automatically render a login page and logout success page for you

AbstractAnnotationConfigDispatcherServletInitializer

The next step is to ensure that the root ApplicationContext includes the HelloWebSecurityConfiguration we just specified. There are many different ways we could do this, but if you are using Spring’s AbstractAnnotationConfigDispatcherServletInitializer it might look something like this: 

    public class SpringWebMvcInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

      @Override
      protected Class<?>[] getRootConfigClasses() {
        return new Class[] { HelloWebSecurityConfiguration.class };
      }
      ...
    }
    
  AbstractSecurityWebApplicationInitializer

The last step is we need to map the springSecurityFilterChain. We can easily do this by extending AbstractSecurityWebApplicationInitializer and optionally overriding methods to customize the mapping.

The most basic example below accepts the default mapping and adds springSecurityFilterChain with the following characteristics:

springSecurityFilterChain is mapped to “/*” springSecurityFilterChain uses the dispatch types of ERROR and REQUEST The springSecurityFilterChain mapping is inserted before any servlet Filter mappings that have already been configured

For other authentication such as jdbc, ldap  please refer  http://www.logicbig.com/tutorials/spring-framework/spring-security/user-details-service/

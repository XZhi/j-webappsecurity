# FAQ #


## Which Web frameworks are supported? ##

J-WebAppSecurity is designed to work transparently regardless of type of Web framework used in an application.

Couple of things to keep in mind: com.remotussoft.jwebappsecurity.filter.SecurityFilter needs to be mapped to URL(s) used to process form submission requests, such as .htm in Spring MVC and .do in Struts, and com.remotussoft.jwebappsecurity.filter.SecurityTokenFilter needs to be mapped to URLs of pages containing forms which J-WebAppSecurity will be protecting such as .jsp and .htm.

## Is there any impact on application performance or browser Back button functionality? ##

Performance impact is negligible.

The framework does not alter application behavior in any way, browser Back button functions normally.

## How do I get notified when CSRF attack is detected? ##

For now J-WebAppSecurity logs CSRF alert to application logging files via SLF4J library at INFO level. Request URL and IP address of the attacker also provided.

## How to enable debug logging ##
J-WebAppSecurity uses SLF4j/LOG4J for logging. To enable logging at DEBUG level insert this line into your "log4j.properties" file:
`log4j.logger.com.remotussoft=DEBUG`

## How to install? ##
To integrate J-WebAppSecurity framework into your Java Web application follow these steps:
**a.** Add the latest J-WebAppSecurity JAR file to your Web application class path. If you use Maven import J-WebAppSecurity JAR into your repository and add the following entry to your Web application pom.xml, replace "version" with the latest J-WebAppSecurity version available:
```
        <dependency>
            <groupId>com.remotussoft</groupId>
            <artifactId>j-webappsecurity</artifactId>
            <version>1.1</version>
        </dependency>
```
**b.** Modify your web.xml to include 2 new filters:
```
    <filter>
        <description>Security token filter</description>
        <filter-name>j-webappsecurity-SecurityFilter</filter-name>
        <filter-class>com.remotussoft.jwebappsecurity.filter.SecurityFilter</filter-class>
    </filter>
    <filter>
        <filter-name>jwebappsecurity-TokenFilter</filter-name>
        <filter-class>com.remotussoft.jwebappsecurity.filter.SecurityTokenFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>jwebappsecurity-SecurityFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
    </filter-mapping>
    <filter-mapping>
        <filter-name>jwebappsecurity-TokenFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
    </filter-mapping>
    <filter-mapping>
        <filter-name>jwebappsecurity-TokenFilter</filter-name>
        <url-pattern>*.jsp</url-pattern>
    </filter-mapping>
```
This example would work for any application using JSP and Spring MVC, since Spring MVC by default uses ".htm" extension to map to controller classes.

Make sure to change mappings to match your application configuration, for example change "**.htm" to "**.do" for Struts framework.

**c.** Modify your JSP pages to include the following scriptlet:
`<%=SecurityUtils.getCSRFTokenScript(request)%>`
This scriptlet should go to the bottom of a JSP.

**d.** Restart/redeploy your Web application and you are all set!

## Configuration ##
To customize the framework behavior add configuration file name parameter to the jwebappsecurity-TokenFilter and place the configuration file in your application classpath:
```
 <filter>
        <filter-name>jwebappsecurity-TokenFilter</filter-name>
        <filter-class>com.remotussoft.jwebappsecurity.filter.SecurityTokenFilter</filter-class>
        <init-param>
          <name>configFile</name>
          <value>jwebappsecurity-config.xml</value>
        </init-param>
</filter>
```
Parameters in the configuration file:

> - **urlExclusions** - list of URLs separated by comma, which will be excluded from CSRF protection. URL patterns supported are same as in Java servlet specification.

> - **tokenTimeout** - timeout value in seconds after which CSRF token will be removed from cache on server so every request with this token will result in CSRF exception thrown. Value -1 means no timeout - token will expire with session. Default -1.

> - **checkReferer** - check optional HTTP Referer header or not, default true.
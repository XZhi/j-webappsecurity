j-webappsecurity is a Java library which addresses common security problems in Web applications.

Currently it offers protection against CSRF (Cross Site Request Forgery) by attaching a long random token to HTML forms and then checking validity of the token received. As additional protection the framework also checks Referer header when present.

j-webappsecurity is designed to act transparently regardless of Web application framework used - code change rarely required which makes it ideal for integrating with existing applications.

The library generates a 20-character random token for each request, the token then attached as a hidden field to every form on a page thus creating protected forms. The name of the hidden field is also randomized for each user session thus making the defense mechanism harder to break.

When protected forms get submitted J-WebAppSecurity checks validity of the token and either allows request to be processed further by Web application or rejects it by throwing ServletException.

Only forms using POST requests are protected. It is advisable for security reasons to NEVER use GET requests and links for submitting application transactions and any state change requests.

#JSON web token (JWT)
- the 2 libraries used to create and verify JWTs in java are:
1. JJWT (from io.jsonwebtoken)
  - the advantage of using JJWT is 
    - Designed specifically for Java
    - Fine-grained claims parsing 
    - easily integrates with Spring Security
    - Actively maintained and used by the spring ecosystem 
2. Auth0 Java JWT (from com.auth0)
- the advantage of using Auth0 Java JWT is 
  - fluent builder pattern
  - Lightweight and easy to use 
  - Great documentation 
  - used outside spring context as well.

## Secret Key
- A secret key is used to sign and verify the JWTs. It should be kept confidential and not exposed to the public.
- The secret key can be a random string or a password, but it should be long enough to ensure security.
- The secret key should be stored securely, such as in an environment variable or a secure vault.
- The secret key should be rotated periodically to ensure security.
- The secret key should be at least 256 bits long for HMAC algorithms.
- there are two common practices/ways for generating a secret key:
  1. Use online key generators such as 
  - https://jwtsecret.com/generate
  2. java code random key generator
## How To Configure JWT in Spring Boot

### Steps to filter a token in each request
1. Create a JWT filter class that extends `OncePerRequestFilter`.
2. Override the `doFilterInternal` method to extract the JWT from the request header. it is an abstract method that is in `OncePerRequestFilter` class.
3. write helper method called "hasAuthorizationBearer" that checks the request header contains a bearer token.
4. write helper method called "getAccessToken" which will extract the token from the request Authorization header.
5. write a method called "getUserDetails" which will extract the user details from the token.
- the return type of this method is `UserDetails` which is an interface in spring security.
6. write a method called "setAuthenticationContext" which will set the authentication context in the security context. this method will let spring determine who the user is and what privileges/roles they have.
7. using the above helper methods, implement the `doFilterInternal` method to extract the JWT from the request header, validate it, and set the authentication context.

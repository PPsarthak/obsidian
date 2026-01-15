---
status: To Review
---
#oauth2 #spring-boot 

**What is OAuth2? (OA2)**
- *Authorization framework* which allows a *third-party application to access a resource* (on behalf of the admin user) *without the admin user's credentials*; admin user is the one who can actually access it
- OA2 does this by generating a token issued by a "provider"

**4 Roles in OA2**:
-  *Resource Server*: The server that holds the resource
- *Resource Owner*: The "admin user" or the one who owns the resource
- *Client*: The "third-party application" trying to access the resource
- *Authorization Server*: The "provider" that issues the OA2 token
#### OAuth2 Flow - Authorization Code Flow
1. The third party application asks for the resource
2. The server redirects the application to OAuth provider to authorize first
3. The application logs in and authorizes himself 
4. A token is generated and sent back to the server
5. Using that token, the required resource is fetched and returned back to the third party application

![[OAuth2 Flow|700]]


> [!faq] Other Flows
> Apart from Authorization Code flow, there are other flows as well
> Implicit Flow - wherein the token is exposed in URLs used to access the resource (FDS Update Controller)
> Resource Owner Password/Credential Flow - wherein the actual credentials of resource owner are revealed

---
References - [OAuth2 in Spring Boot](https://www.youtube.com/watch?v=us0VjFiHogo)
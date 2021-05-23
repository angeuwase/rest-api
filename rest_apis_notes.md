# REST APIs


## API BASICS
### What is an API?
-Application programming interface  
-APIs are about 3 things: communication, abstraction, standardization.  
-An interface is the part of the program that other programs/ pieces of code get to see and interact with because the inner implementation is hidden away. It's basically a set of rules (a contract) that defines how other software programs will communicate with your application.  
-There are different types of API paradigms that define how an API should be designed and implemented.   

### What types of APIs are there?
1. Operating system API  
-When an application runs on an OS, it needs to access certain services from the OS, such as the music player, the file system, the UI to display something, etc. The program communicates with the OS via an API it exposes.  

2. Code Library  
-A library lets you access its features through an API it exposes. It has very strict rules about what you can do and how you need to do it when interacting with its API. The implementation of those features are hidden away from you.

3. Web API (the most common type)  
-Web and mobile aps realy on web APIs to access the backend (database and logic). Web APIs allow web-based applications to share data programmatically over the internet.  
-Web APIs are language/OS/technology agnostic since they use standard internet communication protocols.  

### Why are web APIs important?
1. Web APIs are used by the front-end and mobile apps to access the application server and database.  
2. The API can be used by other companies to extend your application's reach and use cases (ie if they use it to build other products and services that you don't necessarily want to build yourself or cant build yourself).   
3. The API can be monetised in various ways.  
4. The API can be the product, providing clients with seamless interface to a specific functionality.  
5. The API can be part of a company's overall brand strategy as well as a way to generate new leads or upsell another product.  
6. The API can be part of a company's product strategy to increase its usage.  
7. The API can be a way for you to access other services within your own ecosystem or to integrate 3rd party services into your product.  

### How do companies structure their API development?
1. The API as the product  
2. The API for internal developers first  
-It gets released to external developers when the company sees value in making it public.  
-Good thing about this approach is that the API is tried and tested by the time it is made public, but things can get tricky when the needs of internal developers and external developers start to diverge. 
3. The API for external developers first  
-The external developer is always the priority. This means that there is no issue of managing confilcting interests of two groups.  

<hr>

## CONSUMING APIs
-Imports: requests, json
-requests: the object that establishes a request context and makes your http query for you. You give the `get` method it exposes the url of the api endpoint you want to consume and a second optional argument, `params`, which can be a dictionary of your query parameters. It configures the http request for you.    
-json: turns the json response into a python dictionary.  

```
import requests
import json

api_key = "TCLVR8EZUUAAKLXKX0FI"
service_url = 'https://api.mailboxvalidator.com/v1/validation/single?'
parameters = {"email": "11anguwa@gmail.com", "key": api_key }

response = requests.get(service_url, params=parameters)
print(response.json())

```

<hr>

## API PARADIGMS
-An API paradigm is the underlying operation the API is based on.  
-It's important to choose the right paradigm based on current and future needs because once an API is in use, it is very hard and expensive to change, if not impossible.  
-There are several API paradigms, which can broadly be divided into 2 groups: request-response APIs and event-driven.  
-Request–response APIs expose an interface (a set of endpoints) through a web browser. Clients make HTTP requests to those endpoints and the server returns a JSON responses. The three common paradigms used by services to expose request–response APIs are REST, RPC, and GraphQL.  
-Event-driven APIs:   

### Rest APIs
-Stands for Representation State Transfer.  
-It is the most popular and widely used API paradigm. It is used by Google, Twitter, Github.  
-Rest is all about resources. A resource is any item of interest in a given application. It needs to be something that can be identified and operated on. Example for a blog application the resources are users, posts, commends.  
-REST APIs enable the transfer of a representation of a resource's state.   
-State is the current properties of a resource and/or the properties of a resource as a result of an operation performed on the resource.  
-Rest APIs expose resource-based endpoints. HTTP verbs in request messages are used to specify the operation the client wants the server to perform on the resource. The operations can generally be encapulated in a model called CRUD - create, read, update, delete.  
-REST APIs are commonly referred to as RESTful web services because their implementation is based on the HTTP protocol.   
-REST APIs are simple to design, implement and maintain. They are easily understandable because they use standard HTTP methods for operating on resources.  
-Downsides of REST APIs is that when you access a resource you get all the information related to the resource, even data that you don't need, which results in a big payload. You also need multiple API calls to acess to get all the information you need from the API. They are also not optimised for performing operations that fall outside the CRUD model.  

-General rules that REST APIs follow:  
1. REST APIs operate on a client-server relationship, where the API is the server. The client and server are completely separate and indepedent. of each other.   

2. The API server needs to be stateless, meaning that each client request received must contain all the information required to process the request (including authentification credentials) since the server does not store any client information between requests.  

3. The system architecture can be layered, with proxy servers and caches and gateways sitting between the client and the API server to improve performance, reliability, scalability and security.  

4. The endpoints must be well-defined and consistent. The URL should contain:  
a fully qualified domain name or a server IP and port number  
4.1 the api world, to show that you are accessing a different part of the application. This can be done by prefixing the domain name with "api" or making the api world the second field  
eg https://wwww.api.myappname.com/
or https://wwww.myappname.come/api/
4.2 the version of the API being accessed  
4.3 the resource being accessed. Note: Names of resources must be nouns, not verbs  
-Individual resources must have a unique identifier, usually its id in the database.  
-URLs for collections end in a trailing slash  
-Query parameters on a collection of entities can be added to the end of a URL for the collection, and start with a question mark symbol. Each query parameter is given as a key-value pair. Multiple query parameters are concatenated with the & symbol  
-The request with query parameters may return 0 or an infinite number of entities. If no entity is found meeting the criteria its ok. The request with an entities ID must return exactly 1 entity; if no entity meeting that criteria is found it returns an error message   
eg https://www.myapp.com/api/v1/users/ for all the users  
https://www.myapp.com/api/v1/users/17 for user with id 17  
https://www.myapp.com/api/users?startsWith=a

5. Different HTTP verbs invoked on the same endpoint return different results. The HTTP verb specifies to the server what operation to perform on the resource.  
CREATE: creating a new resource. POST method is used. Request message must contain a body. Target is a resource collection URL  
READ: read-only access to the state of a resource. GET method is used. Request message usually doesn't have a body as the parameters are in the URL. Target can be collection or entity url.  
UPDATE: replacing an existing resource. PUT method is used to completely replace a resource (ie overwrite), PATCH is used for partial replacement. Request message must contain a body. Target is an individual url.  
DELETE: deleting an existing resource. DELETE method is used. Request message usually doesn't have a body as the parameters are in the URL. Target is an individual url.  
-An API endpoint doesn't need to support all the HTTP methods. If a client tries to request the server to perform an operation an endpoint doesn't support the response will be a 403 (method not allowed) error.  

6. Request and response messages follow standard HTTP protocol format.  
Request message format:  
method url http_version
headers
blank space
body

Response message format:  
http_version status_code status_message  
headers  
blank space  
body  

```
Example HTTP request to retrieve a charge from the Stripe API:

GET /v1/charges/ch_CWyutlXs9pZyfD
HOST api.stripe.com
Authorization: Bearer YNoJ1Yq64iCBhzfL9HNO00fzVrsEjtVl
----

Example HTTP request to create a charge from the Stripe API:

POST /v1/charges/ch_CWyutlXs9pZyfD
HOST api.stripe.com
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer YNoJ1Yq64iCBhzfL9HNO00fzVrsEjtVl

amount=2000&currency=usd

```

7. REST doesnt specify the format to use to encode resources. However, the standard is to use JSON for web APIs. The `Content-Type` header in requests and responses is used to indicate the format in
which a resource is encoded in the body.  
-It is considered good practice that the client is given a short list of URLs for top-level resources, and then finds subresources through discovery. The way this is achieved is that a JSON response of a resource will include full path URLs to other related resources.   

Example: a blog post resource could have the following
JSON representation:
{
"self_url": "http://www.example.com/api/posts/12345",
"title": "Writing RESTful APIs in Python",
"author_url": "http://www.example.com/api/users/2",
"body": "... text of the article here ...",
"comments_url": "http://www.example.com/api/posts/12345/comments"
}


8. The response message from the server includes standard HTTP status codes that give an indication about the status of the operation.  
Status codes most used for APIs can broadly be divided into catagories:  
2XX: Success 
4XX: Client-side error  
5XX: Server-side error  

200 Okay                    Request executed successfully  
201 Created                 The request has been fulfilled and a new resource created (the response might contain a "Location" header with a full url path to the resource)  
202 Accepted                The request has been accepted and is being processed asynchronously (client doesnt need to wait for completion)  
204 No content              The request was fulfilled and there is no data to return (usually for when updating a large entity)  
400 Bad request             Client sent a request server can't understand  
401 Unauthorized            Client cannot get access to a resource unless they get authorization.  
403 Forbidden               Client went through the authorization process but they don't have sufficient rights to access the resource.  
404 Page not found          A specific resource the client was tryint to access can not be found  
500 Internal server error   Error occurred while trying to process the request. Error message should not give any detailed explaination about what went wrong for security reasons.  

9. The URL of a resource can be written to reflect that a resource only exists within another resource using `:` before the name of the subresource. eg `POST /repos/:owner/:repo/issues`  

### What is a REST API?
-Allow us to fully separate the presentation of content (data) from the content (data) itself   

-REST API: Representational state transfer application programming interface  
-Website: made up of several web pages individually downloaded from the web server. Each web page has an html document containing the content, css stylesheet defining how the content is styled and javascript file manipulating the content or styling or both.  
When a user navigates from one page to another, the client sends a URL request to the server pointing at the desired resource.  The server sends the html document along with its accomponing css and javascript files.  The client then renders the new page from scratch, replacing the entire old content with the new content.  
This approach is ok, but it is very resource intensive. Each new page requires a completed html document, which is written by a developer or CMS, before it is rendered by the client.  
-Web app: A web app runs in the browser and is populated with data from the server. There is a framework for how pages should be displayed. Each page is a view of the current state of the application.  
When a user loads the site for the first time, all of the components are downloaded to the browser, including the html framework, stylesheet and javascript.  
The application sends a URI (uniform resource identifier request) for a web resource representing the next state of the application to be transferred (ie the data required to build the next view).  
When the client receives the response from the server, it uses it to modify, replace or delete existing data so that the webpage can reflect the current state of the application without rendering a whole new page.  
The representational state of the application is transferred as a data object, not a completely new set of files.  
-This allows for the creation of different applications (mobile, web) that all consume the same backend API.  
-An API is a set of rules that define how two pieces of software will interact.    

-URL vs URI:  
URI: any sequence of characters used to identify a resource  
URL: URL identifies the resource but also tells us how to access it by providing an explicit method + location eg http://   
All URLs are URIs but not all URIs are URLs.  

### How REST relates to HTTP
Not all REST APIs use the HTTP protocol but most do. A REST API can run on other protocol. REST and HTTP are not linked, they are just a convinient pair.  
A restful api is rest API that uses the HTTP protocol to grant access to web resources  
HTTP: hypertext transfer protocol. Protocol web browser uses to transer information on the web  

### The 6 constraints of REST  
A rest API is an application programming interface that follows the following 6 architecture design constraints of REST:  
client-server architecture, stateless, cacheable, layered system, code on demand  

1. Client-server architecture
-ensures proper separation of concerns: the client manages the user interface (presentation) and the server manages the data  
-this allows for 1 rest service to serve many different clients that all use the same data but present it differently (scalability)  

2. Statelessness
-the server does not store any information about the client between requests (no sessions, no cookies)  
-each request from a client must be fully complete and contain all the information the server will need to process the request, including authentification and authorization info     
-important for scalability: any server can handle any request  

3. Cacheability
-all rest responses must be clearly marked as cacheable or not cacheable  
-ie the server tells the client "keep this for 5 days then forget about it" etc  
-caching should be blocked for constantly changing responses  
-important for speed  

4. Layered system
-the client shouldnt know or care whether it is connected directly to the server or an intermediary like a CDN or mirror.  
-likewise the server doesnt care who it gets the request from. The server should not try to identify the client using headers in the request, because all requests will be routed through an intermediary (a load balancer)   
-this helps with security and scalability  

5. Code on demand
-servers are allowed to transfer executable code like javascript and compiled components to the clients to extend and customise functionality  
-note: assume that this is optional, because it is not very practical: if the API serves many different clients, how would it know what kind of code a client can execute?  
it makes sense for when the only client is a browser, because if you send it javascript it will be able to execute it  
for scalability, you want your API to be able to serve many different clients   

6. Uniform interface
6.1 resource identification in request  
-A resource is any data/entity of interest in the domain of the application. The resource can be a singleton or a collection.  
-Each resource has a unique identifier, called a URL   
-Each request specifies what resource the client is interested in and it wants the server to take on that resource and then the server returns a representational state of the resource  

6.2 resource manipulation through representations   
-clients dont have direct access to resources, they only see their representations (a copy of the data, presented in a different format).  
-A client performs all of its operations on the respresentations, passes it to the server, and then the server applies the operations to the actual resource. The representation is different to the actual resource  
-the server can provide respresentations of the resource in many different formats called content types    
eg the resource of interest could be stored as a table in the database; but the rest api will return it as a JSON file, not a database table   

6.3 self-descriptive messages  
-the request and response messages are self-contained:   
*the desired operation is given in the http request method   
*the target resource is indicated in the request URL  
*authentification headers provide credentials    
*Content-Type/Accept headers define media types   
*Resource representation is in the body of the message    
*Operation result is indicated by the response status code  

6.4 hypermedia as the engine of application state (HATEOAS)   
-Clients do not know any resource URLs in advance except for the root URL of the API   
-You should have links so that the client discovers new resources through links that it gets from resources that it already knows about.   
-once a client has access to a rest service, it should be able to discover all available resources and methods through the hyperlinks provided in resource representations   

eg request: "give me post #4 in json format"  
response: "methods GET, PUT, DELETE. next post: #5, previous post: #3, category: 2"  


### ACCESS
Clients can be mobile, web app or IoT device: the rest api does not care. it just receives requests, processes them and returns the appropriate response.  
REST APIs can be open, meaning anyone can consume the data that the API exposes.  
Most APIs have strict rules regarding who can access the data and how many requests they can make in a set time period  


### REQUESTS
In REST, any information that can be named can be a resource: a document, an image, a collection of resources, etc  
The resource URI has increased specificity that shows whether the client is interested in a collection or a singleton  
REST APIs allow clients to perform actions on a resource by using the representation to capture the current or intended state of that resource and transferring that representation to the client.  
(ie the client doesnt access the data directly, it access a unique representation or copy of that data)  
resource = whatever data contained at the end of the resource URI  
representation = a unique copy of that data    
A request consists of a METHOD and a resource URI.  
The method is one of the standard http operators that define what action the client wants the server to perform (GET, PUT, POST, DELETE, PATCH).  
The resource URI specifies the resource that the client is interested in and its location  
When you submit a request you can include matadata in the request: Host, Content-Type, Authorization, Cache-Control  
```
GET https://resful.dev/post/5 HTTP/1.1
Host: appsite.dev
Content-Type: application/json
Authorization: Basic dG9t0nBhc3N3b3Jk
Cache-Control: no-cache
```

API discovery: figuring out what resources and methods are available  
A good REST API should have good documentation.  
Even without documentation using the GET and OPTIONS verbs you can navigate your way through the responses of a restful api to create a map of all of its resources and methods  

OPTIONS: returns every available method and resource for a given endpoint  

### RESPONSE 
-Response always has a HEAD section, but that is consumed by the http client and not shown in the response data  
To see the head you send a HEAD request to the API  
-Status: shows the sucess of the request  
1XX information  
2XX success  
3XX redirect  
4XX client error  
5XX server error  

The response you get from a rest API depends on the authorisation level you have  

Authorization:  
*Basic (very insecure)  
*Json web tokens (JWT)  




### Remote Procedure Call (RPC)
-RPC is all about actions. RPC APIs expose action-based endpoints.  
-In an RPC request the client specifies the action it wants the server to take on a given endpoint and the server returns a JSON response.  
-RPC is used where the actions that the client needs to be done don't fit the standard CRUD model.  
-RPC APIs generally follow two simple rules: the endpoint must contain the name of the operation to be executed and the API call must be made with the HTTP verb that is most appropriate: GET for read-only requests and POST for others.  
-RPC-style APIs are not exclusive to HTTP. There are other high-performance protocols that are available for RPC-style APIs, including Apache Thrift and gRPC.   
-Used by Slack, Flickr  
-RPC has high performance, is easy to understand and has light-weight payloads. 
-Downsides of RPC are that discovery is difficult, implementation is not standardized and can lead to function explosion  
-If your API can be encapsulated by the CRUD model, use REST. If it requires actions that don't neatly fit into the CRUD model, use RPC.  

```
Slack’s Conversations API allows several actions, like archive, join, kick, leave, and rename.   
Example of a POST request to Slack’s conversations.archive RPC API to tell it to archive a conversation

POST /api/conversations.archive
HOST slack.com
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer xoxp-1650112-jgc2asDae

channel=C01234

```

### GraphQL
-GraphQL is a query language for the API.  
-In GraphQL clients tell the server how they want the required data to be structured and the server returns a response that mirrors that format. The API traverses and returns application data based on the schema definitions, independent of how the data is stored.  
-GraphQL APIs only need a single endpoint. The endpoint remains the same regardless of the operation you are wanting to perform.   
-GraphQL APIs supports query (GET) and mutation (POST/DELETE/PATCH) operations. In the JSON request body you specify the operation you want to perform. Since the request message has a JSON body, the HTTP operation sent to the API endpoint is always a POST request.  
-It was developed internally by Facebook and then later made public in 2015 and is used by Github, Yelp and Pinterest.  
-The biggest advantages GraphQL has over REST is that the response has a reduced payload since you are allowed to specify exactly what data you want, you only get that data, and you can get all the info you need in a single API call.   
-Other benefits include not requiring versioning (new fields can be added to a GraphQL API without affecting existing fields. Unused fields can also be removed easily).  
-Downside to graphQL is that it has a steep learning curve and is a complex API to build, since the server has to do additional processing to parse complex queries and verify parameters. Optimizing performance of GraphQL queries can be difficult, too, since there are so many unpredictable use cases.  
-If the API you are building is quite simple and straight forward, graphQL is overkill. If you need querying flexibility and a reduced payload, graphQL is the best option.  

```
Example of a GraphQL query:  
The following query looks up the octocat/Hello-World repository, finds the 20 most recent closed issues, and returns each issue's title, URL, and first 5 labels:

query {
  repository(owner:"octocat", name:"Hello-World") {
    issues(last:20, states:CLOSED) {
      edges {
        node {
          title
          url
          labels(first:5) {
            edges {
              node {
                name
              }
            }
          }
        }
      }
    }
  }
}

Example of a GraphQL Mutation:  
Mutations often require information that you can only find out by performing a query first. This example shows two operations:   
A query to get an issue ID.
A mutation to add an emoji reaction to the issue.

query FindIssueID {
  repository(owner:"octocat", name:"Hello-World") {
    issue(number:349) {
      id
    }
  }
}

mutation AddReactionToIssue {
  addReaction(input:{subjectId:"MDU6SXNzdWUyMzEzOTE1NTE=",content:HOORAY}) {
    reaction {
      content
    }
    subject {
      id
    }
  }
}

```

### Choosing an API paradigm

Do you need real-time feedback Y N

Y:




N:
Use request-response paradigm.
Do you need querying flexibility and or a reduced payload Y/N
Y: use graphQL
N: can your application be encapsulated by the CRUD model Y/N
Y: use rest
N: use rpc




<hr>

## API SECURITY

-Security is a critical element of any web application, including APIs, because a security breach can result in theft/loss of critical data as well as loss in revenue.  
-Things engineers do to secure web applications include input validation, using the Secure Sockets Layer (SSL) protocol, validating content types, maintaining audit logs, and protecting against cross-site request forgery (CSRF) and cross-site scripting (XSS).  
-Security specific to APIs includes authenticating and authorizing users before allowing them to access web resources.  
-Authentification is the process of verifying the identity of a user. Web applications usually accomplish this by asking a user to log in with a username/email and password and check these against the credentials stored in the database to ensure that the request is authentic.  
-Authorization is the process of verifying that a user is permitted to do what they are trying to do.   
-Different ways of authenticating a user: session-based authentification, basic http authentification, JWT, OAuth2.0  

## Authentification vs Authorization
Suppose you have an application (PicsArt) that allows ysers to upload images. The images can be uploaded directly from the user's device or from their social media account such as facebook or instagram.  
In that case, PicsArt needs a way to be able to access a user's data on these other platforms.  
To do that, it needs authentification and authorization!  

Authentification (AuthN): the process of verifying the identify of a user to confirm that they are indeed the owner of the given web resources (ie who are you?)    
-the user has to authenticate themself to facebook by providing the correct account login credentials  

Authorization (AuthZ): the process of giving a third party application permission to access your data stored on another platform (ie what can you do?)  
-the user has to authorize PicsArt to have read access to the user's images on facebook.  

Authorization depends on authentification, but they are not interchangeable  

## Basic authorization  
With basic authentification PicsArt would ask the user for their facebook login credentials.  
PicsArt would then access their user's images on facebook on the user's behalf.  

This is terrible for several reasons:  
1. If the user no longer wants to use PicsArt, they would have to change their password to really make sure all the access PicsArt had was revoked, which is massively inconvinient  
2. PicsArt has full access to the user's facebook account, not just access to the images. This is terrible, because the user has no way of restricting the scope of access PicsArt has to their facebook data  
3. Credentials are given to the 3rd party app, which is quite insecure  

## What is OAuth?
Oauth2.0 is an authorization framework. It solves all the problems associated with basic authorization: it is more secure, the 3rd party app doesnt get the user's login credentials, the access the 3rd party app is granted is limited to a defined scope.    
OpenID connect is an extension of OAuth2.0 used for authentification that specifies a standard way of returning user information via a userInfo endpoint  

## Terminology
Resource owner: the owner of the resource the 3rd party client app is trying to access (ie the facebook user)   

Client: the 3rd party application that is tryint to access the protected resource on behalf of the user. It can be a mobile app, desktop app, single page web app, web app, etc (ie PicsArt)   

Resource server: the server that is hosting the protected resource. It has an API that grants access to protected resources to clients using tokens (ie facebook)   

Authorization server: the server which issues access tokens after successfully authenticating a client and the resource owner. the token proves that the resource owner gave the client application permission to access a certain protected resource, and that the client app has registered with the resource server and is a known application. It is normally a separate server/ company to the resource server   

Authorization grant: A credential representing the resource owner's authorization for a client application to access the protected resource. The client application uses the authorization grant to obtain an access token.   

Authorization grant type: refers to the way the client application gets the access token. There are 4 authorization grant types:   
1. implicit grant type: a way for a single-page JavaScript app to get an access token without an intermediate code exchange step. It was originally created for use by JavaScript apps (which don't have a way to safely store secrets) but is only recommended in specific situations (mobile apps or SPAs)   
2. client credentials grant type: Client Credentials grant type is used by clients to obtain an access token outside of the context of a user. This is typically used by clients to access resources about themselves rather than to access a user's resources. (service accounts or microservices where there is no user involved)   
3. resource owner credentials grant type:  suitable in cases where the resource owner has a trust relationship with the client, such as the device operating system or a highly privileged application and The resource owner provides the client with its username and. password  (legacy apps)   
4. authorization code grant type: The authorization server issues the client appp an authorization grant. The client then sends the grant along with the client secret back to the authorization server in order to get the access token. (backend apps)   

Access token: credentials that allow a client application access to a protected resource. It's a string that represents the scope and access length that the resource owner has given to the client app and is enforced by the resource server and authorization server.   

Refresh token: client app receives a refresh token along with an access token. When the access token expires, the client app gives the refresh token to the authorization server in exchange for a new access token.   

Scope: the permissions a client app has regarding a protected resource they have been given access to (ie what can the client application do?). authorization scopes are used to limit the  client app's access to a resource owner's data to only what is required and the resource owner has agreed to.   
-string style: read/ write/ read-write for simple applications   
-URL style: https://api.company.com/resource.write   

When defining scopes:  
-be consistent  
-be granular   

## OAuth endpoints
OAuth only specifies 2 endpoints:   
`/authorize` used for anything user facing. Gets tje authorization grant and user consent. note: resource owner and client credentials grant types dont use this endpoint at all.   
`/token` used to retrieve tokens   

every other endpoint aside from these two is an extension!

## Useful OAuth Extensions
OpenID Connect: provides a way to authenticate users `/userinfo`   
JSON Web Tokens (JWT): JWT are encoded, not encryped; it includes data such as issuer, issued at, subject, audience, expiration date. Data stored in a token is referred to as a 'claim'   
Token revocation: cancelling a token before it expires `/revoke`   
Token introspect: examines a token to determine if the token is valid `/introspect`      
Dynamic client registration and management: consistent API for creating oauth clients. useful in self-service API portals where developers can register on their own.  
Authorization server metadata discovery: allows us to query the authorization server itself and get back a json file with its capabilities and endpoints so we know what extensions it has available for use.  
`/.well-known/oauth-authorization-server`      

Note: aside from `userinfo` endpoint from openID connect, endpoints from extensions can be named anything you want   

## OpenID Connect 
OAuth + addition of ID tokens that contains the user's profile info + userinfo endpoint where you can get user data   

When have you used openID connect?: sign in with google/linkedin/github etc: the client application gets access to your user data and can create a complete user profile from it without you having to provide that info all over again. You simply login to google/linkedin/github to authenticate yourself and authorize the client app, and then your data gets shared with the client app.   


## OAuth Tokens




## How OAuth authorization code grant type works


## Session-based authentification 
-Session-based authentification, also known as cookie-based authentification, is a means of storing user credentials between requests to overcome an HTTP server's stateless nature.  
-When a client first sends a server a request, the client provides user credentials. Once the server has authenticated the user, the server creates a session containing the user's credentials, and stores the session in a cookie that gets sent back to the client in the response. The client saves the cookie in its memory, and sends it to the server with every request. With each request the server can then authenticate the user using the info in the session, without the user having to re-enter their login details.  
-This authentification method is simple and easy to implement.  
-Downsides with this authentification method:  
1. Security: hackers can steal cookies from the user's browser and hijack the session.  
2. Performance: validating the session each time a request is made can be a costly operation if there is a lot of requests.  
3. This authentification method doesn't work well with APIs. It is difficult for non-browser clients to implement session-based authentification.  

## Basic HTTP Authentification 
-The Basic HTTP Authentification method is the most basic technique for enforcing access control on the web.  
-Every request that the client sends contains an `Authorization` header whose value is the word "Basic" followed by a space and a base64 encoded string of the username:password.  
`Authorization: Basic dXNlcjpwYXNzd29yZA==`  
-Although simple to implement, this method offers the least amount of security. Issues with them include:  
1. The user's credentials are not encrypted, but stored in plain text and are easily decoded. This makes them vulnerable to a malicious hacker who could get ahold of them if they are exposed by a bug or some other means.  
2. Once a user grants an application that uses basic authentification access to use their data, they can't revoke access to an individual application. They would need to revoke access to all applications by changing their password.  
3. Applications get full access to user accounts, and users have no way to limit which resources the applications can access.  
access to selected resources.
-Flask has a library, flask-httpauth, that makes it easy to implement basic http authenfication.  

## Token-based Authentification 
-Token-based authentification is the better alternative to basic http authentification, because the user's credentials are encrypted.  
-The client application has to register with the API provider and be given a client id and client secret.  
-The client first makes a request to the server to log the user in and get a signed token. The server authenticates the user and then generates a secure, signed token for the client and sends it in its response message.  
-The client stores the token and every subsequent request that the client sends contains an `Authorization` header whose value is the word "Bearer" followed by a space and the token.  
`Authorization: Bearer 8eb2c5b3a05a8c744c0b4e35f295e095`. When the user logs out of the application the token gets destroyed.  
-There are two types of tokens: access tokens and refresh tokens. When a user is first authenticated, the client gets given both types of tokens.  
-The client has to present the access token to gain access to a protected resource. Access tokens expire after a short time to ensure that even if a hacker gets ahold of it, they will only have access to the account for a short time.  
-When an access token expires the client token presents the refresh token so that they can get a new access token. Reauthentication is not required to receive an access token if you present a refresh token. Refresh tokens also expire, but their lifespans are much longer than that of access tokens.  
-Refresh tokens are quite secure because, to get an access token using a refresh token the client has to present the client id and client secret. Even if a hacker had to get ahold of the refresh tokens they would still need the client id and client secret to use them to get an access token.  
-The benefits of using token-based authentification are:  
1. It is more secure since user credentials are encrypted and tokens expire after some time.  
2. Scalability: servers don't store any user information, so any server can be made available to handle a given client request.  
3. Tokens can be generated anywhere. It is not necessary that the application validating the token be the same one that generated the token. Tokens can be generated on a different server or by a different company.  
4. You can implement authorization with the access token, by specifying what resources it grants the client access to (ie defining scope). Clients will not be able to access unauthorised resources using that access token.  
-The most common method of implementing token-based authentification is JWT. Other methods include Branca, pasito and macaroon.  

## Json Web Tokens (JWT)
-A json web token is a standard that defines a secure and compact way of transmitting user credentials between a client and a server in the form of a json object.  
-The json web token can be signed (base64 encoded), encrypted or both. If it is neither unsigned nor encrypted then it is an ensecure JWT. Signing tokens ensures that its integrity is kept intact (other parties can view its content but cant alter it). Encrypting a token ensures that its security is maintained (other parties cant see the content but they can modify it). It's a good idea to sign and encrypt a token so that its contents cant be seen or modified.  
-A JWT is 3 strings separated by a dot: `encoded_header.encoded_payload.signature`  

Header  
-The header is also known as the JSON object signing and encryption (JOSE). It is a dictionary (that gets base64 encoded) that defines two attributes:  
`alg`, the algorithm used to sign and encrypt the JWT   
`typ`, the content being signed or encrypted  
plain text: `{'alg':'HS256', 'typ': 'JWT'}`
base64 encode: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`  

Payload  
-The payload is a dictionary that contains information that the server uses to identify the user and the permissions they have. The dictionary keys are called `claims`.  
-There are 3 types of claims: registered, public and private claim names  
*registered claim names are reserved keywords:  
`iss`: the principle that issued the jwt  
`sub`: the principle that is the subject of the jwt  
`aud`: the recipient that the jwt is intended for  
`exp`: the expiry date of the jwt  
`nbf`: the time before which the jwt must not be accepted for processing  
`iat`: the time at which the jwt was issued  
`jti`: a unique identifier of the jwt (used to ensure that the same jwt isnt used more than once)  

*public claim names are defined at will by those using the jwt. To prevent collision with the registered claim names, these claims have to be defined in the IANA registry, JSON Web Tokens Registry or be defined as a URI that contains a collision-resistant namespace.  

*private claim names: claims that the producer and consumer of jwt agree to have that are neither reserved nor public. These are subject to collissions so must be used with caution.  

plain text: `{ "sub": "1234567890", "name": "John Doe", "iat": 1516239022}  `  
base64 encode: `eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ`  

Signature  
-The signature is created by combining the base64 encoded header and payload and then hashing them using a secret key and the hashing algorithm specified in the header.  
-The signature can only be decrypted by the same secret key.  
-There are 2 mechanisms for signing tokens: asymmetric signatures and symmetric signatures.  
-With symmetric signatures, the token is signed using a secret key + hashing for message authentification code (HMAC). The secret key used to sign the token is the same one that is used to validate it. The HMAC code is a message authentification code obtained from running a crypotographic function like MD5, SHA1, SHA256 over the data and a security key.  
-The asymmetric signature mechanism is better suited for distributed systems. In this model, there is one server that has a private key. It is in charge of generating tokens, signing them with the private key and then sending them to the client. The client can send the token to any server. The servers validate the token using their public key.  
-The token signature is done using RSA which is an asymmetric encryption and digital signature algorithm.  
-When a server receives a token from a client, it fetches the header and payload parts, then uses the secret key or public key to generate a signature. If the signature it gets matches the one in the JWT then it is considered to be a valid token. Having verifyied the signature, other parts of the token (the claims) are also checked before the token is accepted as legitemate.  
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
your-256-bit-secret

)
```
Result: `cThIIoDvwdueQB468K5xDc5633seEFoqwxjF_xSJyQQ`  

Together: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.cThIIoDvwdueQB468K5xDc5633seEFoqwxjF_xSJyQQ`  





<hr>

## API PERFORMANCE CONSIDERATIONS



Asynchronous operations  
Caching  
rate limiting  
quota  
load balancing  
monitoring and logging  

<hr>




<hr>

## DESIGNG REST APIs


















<hr>

## DEVELOPING REST APIs
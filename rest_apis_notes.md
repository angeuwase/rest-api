# REST APIs

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

## The 6 constraints of REST  
A rest API is an application programming interface that follows the following 6 architecture design constraints of REST:  
1. Client-server architecture
-ensures proper separation of concerns: the client manages the user interface (presentation) and the server manages the data  
-this allows for 1 rest service to serve many different clients that all use the same data but present it differently.  
2. Statelessness
-the server does not store any information about the client between requests  
-each request from a client must be fully complete and contain all the information the server will need to process the request  
-important for scalability: any server can handle any request  
3. Cacheability
-all rest responses must be clearly marked as cacheable or not cacheable  
-ie the server tells the client "keep this for 5 days then forget about it" etc  
-caching should be blocked for constantly changing responses  
4. Layered system
-the client shouldnt know or care whether it is connected directly to the server or an intermediary like a CDN or mirror.  
-this helps with security and scalability  
5. Code on demand
-servers are allowed to transfer executable code like javascript and compiled components to the clients to extend and customise functionality  
6. Uniform interface
6.1 resource identification in request  
-URI must specify what resource the client is interested in  
-the server will return a representational state of the resource  
eg the resource of interest could be stored as a table in the database; but the rest api will return it as a JSON file, not a database table  
6.2 resource manipulation through representations   
-once a client has a representation of a resource ut can modify or delete the resource, given it has the right permissions to do so  
A representation of a resource is a unique copy of that resource, not the resource itself.  
6.3 self-descriptive messages  
-the request and response messages must specify the data they contain eg "i am json"  
6.4 hypermedia as the engine of application state  
-once a client has access to a rest service, it should be able to discover all available resources and methods through the hyperlinks provided  
eg request: "give me post #4 in json format"  
response: "methods GET, PUT, DELETE. next post: #5, previous post: #3, category: 2"  

## How REST relates to HTTP
Not all REST APIs use the HTTP protocol but most do. A REST API can run on other protocol. REST and HTTP are not linked, they are just a convinient pair.  
A restful api is a web service that uses the HTTP protocol to grant access to web resources  
HTTP: hypertext transfer protocol. Protocol web browser uses to transer information on the web  


## ACCESS
Clients can be mobile, web app or IoT device: the rest api does not care. it just receives requests, processes them and returns the appropriate response.  
REST APIs can be open, meaning anyone can consume the data that the API exposes.  
Most APIs have strict rules regarding who can access the data and how many requests they can make in a set time period  



## REST CLIENT
Rest Client in VSCode marketplace  
Can be used to send requests to a restful api and view its responses.  

```
rest.http:

GET https://reqres.in/api/users
```

## REQUESTS
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

## RESPONSE 
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


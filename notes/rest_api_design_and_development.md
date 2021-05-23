# REST API DESIGN AND DEVELOPMENT

## DESIGNG REST APIs
1. You need to understand the business process you are trying to model  eg 'buying a book online'  
*identify the participants (the entities that will use the API)   
-who are they? (name)   
-are they internal or external to the organisation?   
-are they active (taking an action) or passive (waiting for an action)?  

*identify the activities  
should be based on what the users actually need   

*break the activities into steps  

*create API definitions   
-identify the resources   

*validate your API   

2. API design must be deliberate   
-you need to decide what endpoints to expose and what methods they will support   

3. Adding an API:  
*bolt-on strategy: you have an existing application, and you want to make some of its functionality programmatically accessible, so you add on an API.   
-brute force approach. But fast way to get something useful. The funcitionality is already there and you just need to decide how to expose it   
-cons: problems with the application can affect the API   

*greenfield API first/mobile first strategy: you are starting from scratch, and have complete freedom on what to do and how to do it.   
-can use latest tech for implementation   
-cons: you need to make sure requirements are clearly defined so that the API will actually produce real business value   

*facade strategy: take existing applications and shape them to what you need eg wrapping them with a rest API.   

<hr>
<hr>

## Example

Process: ordering a book online   

### Participants:  
customer

*activities: ordering a book online   
assumptions: client only orders one book. will need to figure out process for adding/removing items from the cart at a later stage 

### Steps:   
1. the customer searches for the book   
2. the customer adds the book to their cart   
3. the customer checks out and pays     

### API definitions   
1. identify the resources (use nouns):   books (collection), book (singleton), cart, orders, order   
2. identify the actions you want to perform on each resource  
3. identify the http methods that fit those actions   

books: list books (GET)  
book: view book (GET)    
cart: view cart (GET), create cart (POST), add book to cart (PUT), checkout (POST)   
orders: list orders (GET), view order (GET), cancel order (DELETE) 
order: create order done at point of checkout, view order (GET), cancel order (POST) 

4. identify the relationships between the resources   
*Independent: the resources can exist independently of each other   
*Dependent: one resource can only exist if another resource already exists    
*Associative: can be independent or dependent but needs more information to describe it   

-books are independent   
-cart must have a book  
-order must be for a specific cart  
-order must have a customer   

### API validation
Validate your API before you build it to make sure it will actually work the way you want it to work (cheaper and faster)   
Document it as if the API already exists   

-list the endpoints (what they do)   
-list the parameters (what they mean)  
-list the response code (what activates them)   
-show the response payload (define the fields)   


<hr>
<hr>

## Design Challenges 

*Authentification and authorization: use OAUth 2.0   

*Versioning: in the resource URL   

*Media types: a media type allows us to use a commonly structured json file to transfer data   
collection+json   
hypertext application language (HAL)  hal+json   

*HATEAOS  

*Content negotiation 

*Caching  
-eTag header  
(when client makes a HEAD request, the eTag header will be the same as from the original request, indicating that the data has not changed. if the data has changed since the last request the server will generate a new eTAg for the HEAD request, which will tell the client to make a new request for the resource.)  

*Documentation  


<hr>
<hr>

## EXPLORING A REST AP
https://github.com/miguelgrinberg/api-pycon2015/tree/master/api  I

*navigate to the main API endpoint `GET http://localhost:5000/`   
-if you are running it on your local machine you can use http. If it is deployed you would need to use https   
-the response will indicate that you are not authorised   
-Location header tells you where to go to get the authorization token   
*request an authorization token `GET http://localhost:5000/auth/request-token`   
-response will be Method not allowed   
*run the options request on that endpoint to see what methods it supports   `OPTIONS http://localhost:5000/auth/request-token`
*It supports a POST method because to get a token you need to authenticate by passing it a username and password   
`--auth username POST http://localhost:5000/auth/request-token`   
you will be asked to submit the password for that username   
upon submitting a password you will be sent a token    
*you can now access the API and authenticate with the token
`--auth <token> GET http://localhost:5000/`   
-you will be given a catalogue of all the versions the API supports and the resources you can access  
```
{
versions:   {


    v1: {
        'classes_url':'http://localhost:5000/v1/classes/',
        'students_url':'http://localhost:5000/v1/students/'
    }
}
}

```
*any resource you access will provide links to aid discovery of more resources   
`--auth <token> GET http://localhost:5000/classes/`   
```
HTTP/1.O 200 OK
Cache-Control: max-age=86400
Content-Length: 765
Content-Type: application/json
Date: Fri, 10 Apr 2021 20:41::42 GMT
Etag: `ahahahkaal'
Server: Werkzeug/0.9.4 Python 3.4.3
X-RateLimit-Limit: 5
X-RateLimit-Remaining: 4
X-RateLimit-Reset: 12345

{
    'classes':  [
        'http://localhost:5000/classes/1`,
        'http://localhost:5000/classes/2`,
        'http://localhost:5000/classes/3`,
    ],
    'meta': [
        'first_url': 'http://localhost:5000/classes/?page=1&per_page=10`,
        'last_url': 'http://localhost:5000/classes/?page=1&per_page=10`,
        'next_url': 'http://localhost:5000/classes/?page=1&per_page=10`,
        'page': 1,
        'pages': 2,
        'per_page': 10,
        'prev_url': null,
        'total': 13


    ]
}

```
-meta contains pagination information.   
-
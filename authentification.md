# Web Security

OAuth 2.0   
OpenID Connect   

## Tools
Postman  
Token introspection tool (www.jsonwebtoken.io)  
OAuth server (oauth.com/playground)  

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













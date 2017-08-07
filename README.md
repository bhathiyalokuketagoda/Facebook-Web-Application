# Facebook-Web-Application

We are interacting with lot of web services in our day today life and mostly we are using number of web accounts in different web sites like Facebook, Linked in to EBay, PayPal etc… for each you will have to create a user account or simply you can login with your Facebook, LinkedIn or Google account. this service is provided to you by your application using OAuth which is an Access Delegation Standard. In this article lets see how to create a app to get User Information in Facebook.

First of all lets look at the OAuth standard and how it works. OAuth is an open standard for access delegation, commonly used as a way for Internet users to grant websites or applications access to their information on other websites but without giving them the passwords. This mechanism is used by companies such as Google, Facebook, Microsoft and Twitter to permit the users to share information about their accounts with third party applications or websites.

Following image explains the functionality of OAuth standard.


Image: OAuth Steps
So lets develop and application to login with Facebook authorization and access resources in users Facebook account.

As our first step we have to create a Facebook application. for that you have to visit Facebook Developers website create a application.



Once you complete the first step you get to fill in additional information of your application



In the Dashboard, you can see the App ID and the App Secret for your app. In OAuth terminology, we call the same as Client ID and Client Secret, or Consumer Key and Consumer Secret.



Now we have successfully registered our app in Facebook and configured it.

Then we use above app ID and App Secret to create our application. For demonstration purposes i am using an already developed Java Web Application which i have to change my App ID and App Secret. then compile it using maven and build a .war file. in here you can see How to Install Apache Maven. after that this application can be run on Apache tomcat server.for more about Deploying tomcat server. This application is flow same as the steps in the image: OAuth Steps. Here is the GitHub repository for the Java Web Application.



using this file we can get Authorization code. our client web application needs to send a request to the OAuth authorization endpoint of Facebook. we need to send a HTTP GET request to the Authorize Endpoint of Facebook, which is https://www.facebook.com/dialog/oauth.  Along with the request, you need to send several parameters which are described below

1. Response_type -> Code
2. Client_ID – > The App ID value of your application
3. Redirect_URI -> The Redirection Endpoint URL which you defined in “Facebook Login” settings. This value should be URL encoded when sending with the request.
4. Scopes -> The scopes (permissions to resources) which your app needs to access. When you have multiple scopes, separate them with spaces and the string should be URL encoded when sending with the request.
Refer https://developers.facebook.com/docs/facebook-login/permissions to know more about Facebook scopes.
Sample Request :
https://www.facebook.com/dialog/oauth?response_type=code&client_id=258090368027176&redirect_uri=http%3A//bhathi.ctfboxes.com/&scope=public_profile%20user_posts%20user_friends%20user_photos

So you will be ask to give permission. this page is called User consent page. once you grant the permission Facebook will redirect the browser to the Redirection Endpoint URL, you will get the code as follows.

we need to submit the this Authorization code and should obtain a access token before we access the user resources.
We need to send the following parameters in the body of the HTTP POST request.
1. grant_type -> authorization_code
2. client_id -> The App ID value of your application
3. redirect_uri -> The Redirection Endpoint URL which you defined in “Facebook Login” settings. This value should be URL encoded when sending with the request.
4. code -> The authorization code you received in previous step.
In addition to that, we need to send credentials of the facebook application (App ID and App Secret) in the HTTP Header. Here, we need to combine the App ID and Secret separating them in a Colon (:) and the value should be encoded in Base64.
once you send this HTTP POST Request you will be provided with a access token valid for some period of time with the privileges you Requested/User given. you can save this token and access user data for your application until it expires. once it expires you have to request an new access token.

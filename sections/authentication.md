Image Relay API Authentication
==============================

API Key Auth
-------
Most integrations should use an API Key to authenticate with the Image Relay API. Please be aware that API Keys are scoped to the user that created it, and not the organization. This means that the ability to read certain data and perform certain actions is determined by the API Key creator's permission and access level within Image Relay.

##### Generating an API Key

1. You can create API Keys inside of Image Relay. You need a paid Image Relay account to do this. Once you are logged in to IR, click on "My Account" in the upper right corner. Select "API Keys" from the menu on the left. From here you can create, delete, disable and enable API Keys.

2. Note: You cannot retrieve the API Key again once it is generated for security purposes. If you lose it, however, you can generate another one and disable/delete the lost key. Keep this in mind when managing your keys.

##### Making requests with an API Key

Once you've generated an API key and copied it somewhere safe, you can use that key to authenticate with the Image Relay API by including an __Authorization__ header with your API requests like this:

```
Authorization: Bearer <your_api_key>
```

Basic Auth
----------

To just get started and play with Image Relay's API, you can use HTTP Basic authentication with your own username and password:

```shell
curl -u username:password -H 'User-Agent: MyApp (yourname@example.com)' https://[your_company].imagerelay.com/api/v2/folders.json
```
- _Do not use Basic Auth in Production environments_
- _You should never ask other users for Image Relay usernames and passwords or store them anywhere_
- _For deployed applications that **require you to authenticate other IR users in your organization**, you should use OAuth2 instead of API Key or Basic authentication. More info on OAuth2 can be found below_

OAuth 2
-------
If you are integrating the Image Relay API into a web application that will authenticate IR users, and make requests on their behalf, you should implement an OAuth Flow.

*Image Relay uses the OAuth2 protocol to authorize requests to the Image Relay API. We conform to the [draft-10](http://tools.ietf.org/html/draft-ietf-oauth-v2-10) standard.*

##### How to Setup OAuth2

1. Register your application with Image Relay. You need an Image Relay account to do this. Once logged in to IR, click on "My Account" in the upper right corner. Select "Developers" from the menu on the left. You'll need to provide your application name and a callback URI. Please note - you need a __paid__ Image Relay account to do this.

 - The callback URI specified in the configuration must point to a web service or something that is capable of receiving a web request. The request will contain a code that you will use to exchange for an authorization token. https://webhook.site/ is a nice alternative if you are setting this up for the first time. **NOTE** If you use the webhook site, make sure to copy the URL from the page, _not your address bar_.

2. To begin the process of obtaining an OAuth token, visit the authorization endpoint in a web browser - https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/oauth/authorize...... Below is an example of the full constructed URL.

        https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/oauth/authorize?response_type=code&client_id=<YOUR_CLIENT_ID>&redirect_uri=https://<YOUR_REDIRECT_URI>&state=<RANDOM_STRING>

3. Upon visiting that URL you will need to login. Once you are logged in, Click the 'Yes give them access button' to grant access and then you will be redirected to the redirect uri that you specified when configuring the application (it should match the redirect_uri param in the url from step 2)

4. Your web service will have received a request with a `code` parameter and value. If you're using webhook.site, this will be the first request made on the left hand panel. Select that, then get the ID Token value from the request body.

5. Now use the code that your web service received in step 4 to obtain an access token. You can use CURL to perform this request or some other API testing tool of your choosing.

        POST https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/oauth/token?client_id=<YOUR_CLIENT_ID>&redirect_uri=<YOUR_REDIRECT_URI>&client_secret=<YOUR_CLIENT_SECRET>&code=<AUTH_CODE_FROM_STEP_4>&grant_type=authorization_code

6. Your app can now use the access token you got back from the `POST` request in step 5 to make authorized requests to the Image Relay API. You use the access token in the request by setting the Authorization request header:

        Authorization: OAuth THE_ACCESS_TOKEN

7. Once you get an access token, try it out, you can make a request to get information about the user that just authorized you, by making an authenticated request to

		GET https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/api/v2/users/me.json


End Points
----------

* `GET https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/oauth/authorization`
* `POST https://<YOUR_IR_SUBDOMAIN>.imagerelay.com/oauth/token`

Implementations
---------------
If you are developing your app using ruby, Image Relay has released an omniauth strategy for the IR API. You can find it here: https://github.com/imagerelay/omniauth-imagerelay

We know OAuth2 can be a bit tricky when you are getting started, if you have questions, please e-mail support@imagerelay.com; include your IR URL in your request.

The Image Relay API
===================

The [Image Relay](https://imagerelay.com) API allows IR users to access many Image Relay features through a JSON RESTful web interface.

Making a Request
----------------

All requests start with https://[company url].imagerelay.com/api/v2/

In curl a request would look like this:

```shell
curl -u user:pass -H 'User-Agent: MyApp (yourname@example.com)' https://api.imagerelay.com/api/v2/folders.json
```
If you are sending data to the API, include the Content-Type header and the data as JSON.

```shell
curl -u username:password \
  -H 'Content-Type: application/json' \
  -H 'User-Agent: MyApp (yourname@example.com)' \
  -d '{ "name": "New Catalog" }' \
  https://api.imagerelay.com/api/v2/folders/555/children.json
```
Authentication
--------------

To test things out and get started, you can use http basic authentication with the IR API. In production, you should use the supported oauth2 authentication. That way you don't have to worry about storing Image Relay usernames' and passwords' in your own application. Click [here] (https://github.com/imagerelay/API/blob/master/sections/authentication.md) for more detailed information regarding oauth authentication.


Identify your app
-----------------

You must include a `User-Agent` header with the name of your application and a link to it or your email address so 
we can get in touch in case you're doing something wrong or something great. Here's an example:

    User-Agent: MyCompany (http://www.mycompany.com)

If you don't supply this header, you will get a `400 Bad Request`.

Rate limiting
-------------

You can perform up to 5 request/second from the same IP address. If you exceed this limit, you'll get a [429 Too Many Requests](http://tools.ietf.org/html/draft-nottingham-http-new-status-02#section-4) response for subsequent requests.

APIs
-------------------------------------------------------------------

* [File Types](https://github.com/imagerelay/api/blob/master/sections/file_types.md)
* [Files](https://github.com/imagerelay/api/blob/master/sections/files.md)
* [Folder Links](https://github.com/imagerelay/api/blob/master/sections/folder_links.md)
* [Folders](https://github.com/imagerelay/api/blob/master/sections/folders.md)
* [Invited Users](https://github.com/imagerelay/api/blob/master/sections/invited_users.md)
* [Keyword Sets/Keywords](https://github.com/imagerelay/api/blob/master/sections/keywording.md)
* [Permissions](https://github.com/imagerelay/api/blob/master/sections/permissions.md)
* [Quick Links](https://github.com/imagerelay/api/blob/master/sections/quick_links.md)
* [Uploads](https://github.com/imagerelay/api/blob/master/sections/uploads.md)
* [Upload Links](https://github.com/imagerelay/api/blob/master/sections/upload_links.md)
* [Users](https://github.com/imagerelay/api/blob/master/sections/users.md)
* [Webhooks](https://github.com/imagerelay/api/blob/master/sections/webhooks.md)

Libraries
---------
* [Image Relay PHP Library](https://github.com/imagerelay/imagerelay-php) 

Errors
------
The actions you can access in the API are dependant upon the permission levels assigned to your Image Relay account.
For instance, not all users are permitted to upload files or create folders, or see a list of users. If you find yourself
receiving "401 Unauthorized" errors, please confirm your permission level with your Image Relay Administrator.
 
If you find a typo or an error in the documentation, we welcome pull requests.

If you have questions or trouble implementing the API, you can reach out to support@imagerelay.com and we'll help you out.
Need general help with Image Relay? Checkout our [online support center](http://support.imagerelay.com).

Want to Chat?
-------------
Need help, have questions? Check out our developers [slack channel](https://imagerelay-dev-slackin.herokuapp.com/).




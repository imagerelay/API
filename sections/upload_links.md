Upload Links
===========

Upload links allow you to quickly share an easy way for someone to upload files to your Image Relay library.

Get Upload Links
---------------

* `GET /upload_links.json` will return `200 OK` the authenticated user's list of upload links

We will return 100 upload links per page. If the result set has 100 upload links, it's your responsibility to check the next page to see if there are any more upload links -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
    {
        "id": "<upload_link_id1>",
        "purpose": "testing upload link",
        "user_id": "<user_id>",
        "uid": "<uid>",
        "expires_on": null,
        "created_at": "2015-06-05T19:32:10Z",
        "updated_at": "2015-06-05T19:32:10Z",
        "folder_id": "<folder_id1>",
        "upload_link_url": "<upload_link_url1>"
    },
    {
        "id": "<upload_link_id2>",
        "purpose": "testing upload link",
        "user_id": "<user_id>",
        "uid": "<uid>",
        "expires_on": null,
        "created_at": "2015-06-05T19:32:10Z",
        "updated_at": "2015-06-05T19:32:10Z",
        "folder_id": "<folder_id2>",
        "upload_link_url": "<upload_link_url2>"
    }
]
```

Get Upload Link
--------------

* `GET /upload_links/<upload_link_id>.json` will return `200 OK` and the specified upload link.

```json
{
    "id": "<upload_link_id>",
    "purpose": "testing upload link",
    "user_id": "<user_id>",
    "uid": "<uid>",
    "expires_on": null,
    "created_at": "2015-06-05T19:32:10Z",
    "updated_at": "2015-06-05T19:32:10Z",
    "folder_id": "<folder_id>",
    "upload_link_url": "<upload_link_url>"
}
```

Create Upload Link
-----------------

* `POST /upload_links.json` will create a new upload link for the specified folder. `purpose` is defined by you and can be whatever you like.

```json
{
  "purpose":"Testing API",
  "folder_id":"<folder_id>",
  "expires_on":""
}
```

This will return `201 Created`, if successful, as well as a representation of the upload link.

```json
{
  "id": "<upload_link_id>",
  "purpose": "Testing API",
  "user_id": "<user_id>",
  "uid": "<uid>",
  "expires_on": null,
  "created_at": "2015-06-09T15:06:33Z",
  "updated_at": "2015-06-09T15:06:33Z",
  "folder_id": "<folder_id>",
  "upload_link_url": "<upload_link_url>"
}
```


Delete Upload Link
-----------------

* `DELETE /upload_links/<upload_link_id>.json` will delete the specified upload link.

This will return `204 No Content`, if successful.

Webhooks
=========

Webhooks allow you to receive notifications at an end-point of your choice for a myriad of events in Image Relay. 
Get notified when a file is uploaded, a new user is added, a folder is deleted...

When there is a problem with a webhook, Image Relay send an email to the email of the user the webhook was created 
by. There is also support for adding additional notification_emails if you wish to alert other email addresses of issues.

Get Webhooks
-----------

* `GET /webhooks.json` returns a list of all of your webhooks
* `GET /webhooks/51.json` returns details about the webhook with id 51.

```json
[
    {
        "user_id":405,
        "resource":"file",
        "action":"created",
        "url":"http://example.com",
        "created_at":"2015-06-18T19:18:35Z",
        "notification_emails": [
          "email@example.com", 
          "email2@example.com"
        ]
    },
    {
        "user_id":405,
        "resource":"file",
        "action":"expiration_date_set",
        "url":"http://example.com",
        "created_at":"2015-06-18T19:18:35Z",
        "notification_emails": [
          "email@example.com", 
          "email2@example.com"
        ]
    }
]
```

Create Webhook
--------------


* `POST /webhooks.json` will create a new webhook.

When you create a webhook, when the event occurs that is specified in the webhook, Image Relay will POST
the event details back to you at the URL specified in the webhook.

```json
{
  "resource": "file",
  "action": "created",
  "url": "https://example.com",
  "notification_emails": [
    "email1@example.com",
    "email2@example.com"
  ]
}
```

This will return `201 Created`, if successful.

Update Webhook
--------------


* `PUT /webhooks/19.json` will update a webhook.
```json
{
	"state":"paused"
}
```

Will return `200 OK` and a representation of the keyword set. Valid values for "state" are: normal, error, paused

```json
{
  "resource": "file",
  "action": "created",
  "url": "https://example.com",
  "state": "paused",
  "notification_emails": [
    "email1@example.com",
    "email2@example.com"
  ]
}
```

Delete Webhook
--------------

* `DELETE /webhooks/51.json` will delete the webhook where 51 represents the id of the webhook you wish to delete.

This will return `204 No Content` if successful.

Supported Webhooks
------------------

* `GET /webhooks/supported.json` returns a list of webhooks that Image Relay currently supports, these include the resource
and action required to create the hook. Authentication is not required for this call. Check it out here: [https://api.imagerelay.com/api/v2/webhooks/supported.json](https://api.imagerelay.com/api/v2/webhooks/supported.json)

This will return `200 Ok`

```json
[
  {
    "resource":"file",
    "supported_actions":
    [
        "created",
        "expiration_date_set",
        "expiration_date_removed",
        "file_type_changed",
        "name_changed",
        "recovered",
        "nested_file_deleted",
        "added_to_folder",
        "removed_from_folder",
        "keywords_updated",
        "updated_file",
        "processed"
    ]        
  },
  {
    "resource":"folder",
    "supported_actions":
    [
      "created",
      "renamed",
      "moved",
      "deleted",
      "recovered",
      "shared"
    ]
  },
  {
    "resource":"user",
    "supported_actions":
    [
      "created",
      "modified",
      "destroyed"
    ]
  }
]
```


        "expiration_date_set",
        "expiration_date_removed",
        "file_type_changed",
        "name_changed",
        "recovered",
        "added_to_folder",
        "removed_from_folder",
        "keywords_updated",
        "updated_file",


Details about our currently supported list of resources and actions:
--------------------------------------------------------------------

###Response

All Webhook POSTs will contain the following information in JSON format:

```json
{
    "event": {
            "resource":"file",
            "action":"added_to_folder",
            "resource_id":242694,
            "actor_id":405,
            "recipient_id":11285
    },
    "data": { "id":1234 }
}
```

In the "event" section, resource is the type of resource triggering the event (e.g. file, folder...), action is the
type of action that occurred (e.g. created, updated...), resource_id is the unique id in Image Relay for the resource
that triggered this event. actor_id is the unique id of the user who caused the event to happen (i.e. uploaded the file, 
created the folder), and recipient_id is optionally present when necessary. For instance, in the case of a "File added to
Folder" triggered, recipient_id would be the unique folder id of the folder to which the file was added.

The data section contains data specific to the resource being triggered. For a File, for instance it will contain details
about the file, for a user, it will contain details about the user and so forth. The "event" section will always be present
for all webhook calls.

###Resource: File

* _Created:_ triggered when a file is created at Image Relay
* _Processed:_ triggered when a file has been created or updated in Image Relay and is available for download
* _Expiration Date Set:_ triggered when a user sets an expiration date on a file (NOT when the file expires)
* _Expiration Date Removed:_ triggered when a user removes an expiration date from a file (NOT when the file expires)
* _File Type Changed:_ triggered when a user changes the File Type of a file
* _Name Changed:_ triggered when a user changes the name of a file
* _Recovered:_ triggered when a file is recovered from the trash
* _Added To Folder:_ triggered when a file is added to a folder
* _Removed from Folder:_ triggered when a file is removed from a folder
* _File Updated_: triggered when the original file is updated in Image Relay

####Sample Data

```json
{
    "event": {
            "resource":"file",
            "action":"added_to_folder",
            "resource_id":242694,
            "actor_id":405,
            "recipient_id":11285
    },
    "data": {
            "content_type":"image/png",
            "created_at":"2015-06-18T15:20:00Z",
            "delete_user_id":null,
            "deleted":null,
            "expires_on":null,
            "height":null,
            "id":242694,
            "size":360338,"
            updated_on":"2015-06-18T15:20:00Z",
            "user_id":405,
            "width":null,
            "file_type_id":149,
            "terms":[],
            "folders":["Buffy"],
            "folder_ids":[11285],
            "webdav_paths":["/webdav/Buffy/IMG_0983.png"],
            "permission_ids":[181,177,1,169,236,271,269,184,161]
    }
}
```

###Resource: Folder

* _Created:_ triggered when a folder is created at Image Relay
* _Renamed:_ triggered when a folder is renamed
* _Moved:_ triggered when a folder is moved
* _Deleted:_ triggered when a folder is deleted
* _Recovered:_ triggered when a folder is recovered
* _Shared:_ triggered when a user shares a public link to a folder


####Sample Data

```json
{
    "event":
    {
        "resource":"folder",
        "action":"created",
        "resource_id":12449,
        "actor_id":405
    },
    "data":
    {
        "created_on":"2015-06-23T16:55:11Z",
        "id":12449,
        "name":"My Folder",
        "parent_id":6527,
        "updated_on":"2015-06-23T16:55:11Z",
        "user_id":405,
        "full_path":"/My Folder",
        "child_count":0,
        "file_count":0,
        "webdav_path":"/webdav/My Folder",
        "permission_ids":[1]
    }
}
```

###Resource: User

* _Created:_ triggered when a user is created at Image Relay
* _Modified:_ triggered when a user is modified
* _Destroyed:_ triggered when a user is destroyed

####Sample Data

```json
{
    "event":{
        "resource":"user",
        "action":"created",
        "resource_id":1967,
        "actor_id":405
    },
    "data":{
        "company":"Vampires R Us",
        "created_on":"2015-06-23T17:03:03Z"
        ,"custom_field_four":null,
        "custom_field_one":null,
        "custom_field_three":null,
        "custom_field_two":null,
        "email":"buffy_summers@example.com",
        "first_name":"Buffy",
        "id":1967,"
        last_name":"Summers",
        "login":"buffy_summers@example.com",
        "updated_on":"2015-06-23T17:03:03Z",
        "portal_id":null,
        "permission_id":236
    }
}
```










        
        
        
        
        

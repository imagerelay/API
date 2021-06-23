Webhooks
=========

Webhooks allow you to receive notifications at an end-point of your choice for a myriad of events in Image Relay.
Get notified when a file is uploaded, a new user is added, a folder is deleted, etc.

When there is a problem with a webhook, Image Relay will send an email to the user that created the webhook.
The API also supports optionally adding additional notification_emails to the webhook if you wish to
alert other email addresses if issues arise.


Get Webhooks
-----------

* `GET /webhooks.json` returns a list of all of your webhooks
* `GET /webhooks/<webhook_id>.json` returns details about the specified webhook.

```json
[
  {
    "user_id":"<user_id>",
    "resource":"<resource1>",
    "action":"<supported_action1>",
    "url":"<url_posting_to1>",
    "created_at":"created",
    "state": "normal",
    "notification_emails": ["<notification_email>"]
  },
  {
    "user_id":"<user_id>",
    "resource":"<resource2>",
    "action":"<supported_action2>",
    "url":"<url_posting_to2>",
    "created_at":"created",
    "state": "normal",
    "notification_emails": ["<notification_email>"]
  }
]
```

Create Webhook
--------------

Image Relay webhooks all include a `resource` and an `action`. Supported resources and their actions can be viewed [below](https://github.com/imagerelay/api/blob/master/sections/webhooks.md#supported-webhooks)

* `POST /webhooks.json` will create a new webhook.

After a webhooks has been created, if an event occurs that is specified in the webhook, Image Relay will POST the event details back to you at the `url` specified in the webhook.

```json
{
  "resource": "<resource>",
  "action": "<supported_action>",
  "url": "<url_to_post_to>",
  "notification_emails": ["<notification_emails>"]
}
```

This will return `201 Created`, if successful.

Update Webhook
--------------

* `PUT /webhooks/<webhook_id>.json` will update the specified webhook's state.


Valid `state` values are:
* `normal`: normal functionality.
* `paused`: user-initiated paused state, webhook will not be invoked
* `error`: Image Relay encountered too many HTTP errors when invoking the webhook callback URL, callbacks have been temporarily suspended until the next day when it will be auto-resumed.

```json
{
	"state":"<new_state>"
}
```

Will return `200 OK` and a representation of the webhook.
```json
{
  "resource": "<resource>",
  "action": "<supported_action",
  "url": "<url_to_post_to>",
  "state": "paused",
  "notification_emails": null
}
```

Delete Webhook
--------------

* `DELETE /webhooks/<webhook_id>.json` will delete the specified webhook.

This will return `204 No Content` if successful.

Supported Webhooks
------------------

* `GET /webhooks/supported.json` returns a list of webhooks that Image Relay currently supports, these include the resource
and action required to create the hook. Authentication is not required for this call. Check it out here: [https://api.imagerelay.com/api/v2/webhooks/supported.json](https://api.imagerelay.com/api/v2/webhooks/supported.json)

This will return `200 Ok`

```json
[
  {
    "resource": "file",
    "supported_actions": [
      "created",
      "expiration_date_set",
      "expiration_date_removed",
      "file_type_changed",
      "name_changed",
      "recovered",
      "added_to_folder",
      "removed_from_folder",
      "keywords_updated",
      "updated_file",
      "processed",
      "deleted"
    ]
  },
  {
    "resource": "folder",
    "supported_actions": [
      "created",
      "renamed",
      "moved",
      "deleted",
      "recovered",
      "shared"
    ]
  },
  {
    "resource": "user",
    "supported_actions": [
      "created",
      "modified",
      "destroyed"
    ]
  }
]
```

Details about our currently supported list of resources and actions:
--------------------------------------------------------------------

### Response

All Webhook POSTs will contain an `event` section which then has a `resource`, `action`, `resource_id`, `actor_id`, and potentially `recipient_id`. `resource` is the type of resource triggering the event (e.g. file, folder...), `action` is the type of action that occurred (e.g. created, updated...), `resource_id` is the unique id in Image Relay for the resource that triggered this event. `actor_id` is the unique id of the user who caused the event to happen (i.e. uploaded the file,
created the folder), and `recipient_id` is optionally present when necessary. For instance, in the case of a "File added to
Folder" triggered, `recipient_id` would be the unique folder id of the folder to which the file was added. The `event` section will always be present for all webhook calls.

A `data` section is also included containing data specific to the resource being triggered. For a File, for instance, it will contain details
about the file such as name, id, parent folder, etc.

### Resource: File

* _Created:_ triggered when a file is created at Image Relay
* _Deleted:_ triggered when a file is deleted from Image Relay
* _Processed:_ triggered when a file has been created or updated in Image Relay and is available for download
* _Expiration Date Set:_ triggered when a user sets an expiration date on a file (NOT when the file expires)
* _Expiration Date Removed:_ triggered when a user removes an expiration date from a file (NOT when the file expires)
* _File Type Changed:_ triggered when a user changes the File Type of a file
* _Keywords/Tags Updated:_ triggered when a file's tags are updated
* _Name Changed:_ triggered when a user changes the name of a file
* _Recovered:_ triggered when a file is recovered from the trash
* _Added To Folder:_ triggered when a file is added to a folder
* _Removed from Folder:_ triggered when a file is removed from a folder
* _File Updated_: triggered when the original file is updated in Image Relay

#### Sample Data

```json
{
  "event": {
    "resource":"file",
    "action":"<file_action>",
    "resource_id":"<file_id>",
    "actor_id":"user_id>",
    "recipient_id":"<potential_folder_id>"
  },
  "data": {
    "id": "<file_id>",
    "content_type": "application/octet-stream",
    "size": "<size>",
    "width": null,
    "height": null,
    "created_at": "<created>",
    "updated_on": "<updated>",
    "deleted": true/false,
    "user_id": "<user_id>",
    "expires_on": null,
    "name": "<filename>",
    "file_type_id": "<file_type_id>",
    "terms": [
      {
        "name": "<term>",
        "value": "",
        "metaterm_id": "<metaterm_id>"
      },
      {...}
    ],
    "folders": ["<folders>"],
    "folder_ids": ["<folder_ids>"],
    "webdav_paths": ["/webdav/<path_to_resource>"],
    "permission_ids": ["<permission_ids"]
  }
}
```

### Resource: Folder

* _Created:_ triggered when a folder is created at Image Relay
* _Renamed:_ triggered when a folder is renamed
* _Moved:_ triggered when a folder is moved
* _Deleted:_ triggered when a folder is deleted
* _Recovered:_ triggered when a folder is recovered
* _Shared:_ triggered when a user shares a public link to a folder


#### Sample Data

```json
{
    "event":
    {
        "resource":"folder",
        "action":"<folder_action>",
        "resource_id":"<folder_id>",
        "actor_id":"<user_id>"
    },
    "data":
    {
        "created_on":"<created>",
        "id":"<folder_id>",
        "name":"<folder>",
        "parent_id":"<parent_folder_id>",
        "updated_on":"<updated>",
        "user_id":"<user_id>",
        "full_path":"<path_from_root>",
        "child_count":0,
        "file_count":0,
        "webdav_path":"/webdav/<folder>",
        "permission_ids":["<permission_ids>"]
    }
}
```

### Resource: User

* _Created:_ triggered when a user is created at Image Relay (not just invited)
* _Modified:_ triggered when a user is modified
* _Destroyed:_ triggered when a user is destroyed

#### Sample Data

```json
{
    "event":{
        "resource":"user",
        "action":"<user_action>",
        "resource_id":"new_user_id",
        "actor_id":"<main_user_id>"
    },
    "data":{
        "company":"<new_user_company>",
        "created_on":"<created>"
        ,"custom_field_four":null,
        "custom_field_one":null,
        "custom_field_three":null,
        "custom_field_two":null,
        "email":"<new_user_email>",
        "first_name":"<first_name>",
        "id":"<new_user_id>",
        "last_name":"<last_name>",
        "login":"<login>",
        "updated_on":"<updated>",
        "portal_id":null,
        "permission_id":"<permission_group_id>"
    }
}
```

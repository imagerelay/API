Collections
===========

Allows you to retrieve, create, update, and delete your collections.

Get Collections
----------
* `GET /collections.json` will return `200 OK` and a list of collections created by the authenticated user.
We will return 100 collections per page. If the result set has 100 collections, it's your responsibility to check the next page to see if there are any more collections -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id": "<collection_id>",
    "name": "My Collection",
    "description": null,
    "client_id": "<client_id>",
    "user_id": "<user_id>",
    "created_on": "2017-11-14T16:02:04.000Z",
    "updated_on": "2019-08-14T13:43:19.000Z",
    "lowres_uid": null,
    "data_export": "",
    "hires_uid": null,
    "deleted_at": null,
    "archived": false,
    "theme_name": "showcase",
    "sorting": "custom",
    "metadata": {
      "public_views": 1
    },
    "modified_at": "2019-08-14T13:42:18.000Z"
  },
  {
    ...
  }
]
```

Get collection
---------
* `GET /collections/<collection_id>.json` will return `200 OK` and a JSON representation of the specified collection.

```json
{
  "id": "<collection_id>",
  "name": "My Collection",
  "created_on": "2017-11-14T16:02:04.000Z",
  "updated_on": "2019-08-14T13:43:19.000Z",
  "urls": [
    {
      "level": "high_res",
      "description": "High Resolution",
      "url": "https://your-domain.imagerelay.com/sb/high-res-id/my-collection"
    },
    {
      "level": "med_res",
      "description": "Medium Resolution",
      "url": "https://your-domain.imagerelay.com/sb/med-res-id/my-collection"
    },
    {
      "level": "low_res",
      "description": "Low Resolution",
      "url": "https://your-domain.imagerelay.com/sb/low-res-id/my-collection"
    },
    {
      "level": "no_res",
      "description": "View Only",
      "url": "https://your-domain.imagerelay.com/sb/no-res-id/my-collection"
    }
  ]
}
```

Get collection files
---------
* `GET /collections/<collection_id>/files.json` will return `200 OK` with a list of files associated with the collection.
We will return 100 files per page. If the result set has 100 files, it's your responsibility to check the next page to see if there are any more files -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id":"<file_id>",
    "filename":"<filename>",
    "created_at":"2013-05-20T12:58:07Z",
    "updated_on":"2013-05-20T13:03:36Z",
    "size":7358,
    "width":101,
    "height":101,
    "content_type":"image/png",
    "user_id":"<user_id>",
    "deleted":null,
    "deletion_date":null,
    "delete_user_id":null,
    "file_type_id":"<file_type_id>",
    "terms":
      [
        {
          "term_id":"<term_id1>",
          "value":" "
        },
        {
          "term_id":"<term_id2>",
          "value":" "
        },
        {
          "term_id":"<term_id3>",
          "value":" "
        },
        {
          "term_id":"<term_id4>",
          "value":" "
        }
      ],
      "tags":
      [
        {
          "tag_id":"<tag_id1>",
          "value":"Sports"
        },
        {
          "tag_id":"<tag_id2>",
          "value":"Marketing"
        }
      ]
  },
  {
    ...
  }
]
```

Create collection
--------------

* `POST /collections.json` will create a new collection. `name` is required and `asset_ids` is an optional comma separated list of IDs of assets/files to be added to the new collection.

```json
{
  "name": "New Collection Name",
  "asset_ids": "<asset_id1>, <asset_id2>, <asset_id3>"
}
```

This will return `201 Created`, if successful.

Update collection
--------------

* `PUT /collections/<collection_id>.json` will update an existing collection. `name` is required and `asset_ids` is an optional comma separated list of IDs of assets/files to be *added* to the new collection.

```json
{
  "name": "New Collection Name",
  "asset_ids": "<asset_id1>, <asset_id2>, <asset_id3>"
}
```

Will return a `200 OK` response and a JSON representation of the collection.

Delete collection
--------------

* `DELETE /collections/<collection_id>.json` will delete the specified collection.

Will return a `204 No Content` response if the collection was deleted successfully.

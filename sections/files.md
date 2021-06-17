Files
=====

Files - the reason you're using Image Relay. Here's how you get at them via the API.

Get Files
---------

* `GET /folders/<folder_id>/files.json` returns all the files in the specified folder.

We will return 100 files per page. If the result set has 100 files, it's your responsibility to check the next page to see if there are any more files -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id":"<file_id1>",
    "filename":"<filename1>",
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
    "id": "<file_id2>",
    "filename":"<filename2>",
    "created_at":"2013-05-20T12:58:05Z",
    "updated_on":"2013-05-20T13:03:33Z",
    "size":2974,
    "width":15,
    "height":19,
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
  }
]
```

You may limit files returned by keyword using query parameters, for example:
`/api/v2/folders/<folder_id>/files.json?<query_param>=dogs`

To search all files in your library by keyword via the API, obtain the root folder ID (`GET /folders/root.json`). This ID will never change, then call:
`/api/v2/folders/<root_folder_id>/files.json?<query_param>=dogs&recursive=true`

The following filters/query parameters (*<query_param>*) can be used when requesting a folder's files:

* `uploaded_after` - filters the files returned to only those that have been uploaded after that date. The format of the parameter should be: "YYYY-MM-DD HH:MM:SS GMT+00:00". You can leave off the timezone information if you are using UTC time, you can leave off time if you want files filtered from the beginning of the day specified (e.g. YYYY-MM-DD)

* `updated_after` - filters the files returned to only those that have been updated/modified after that date. The date/time should be formatted the same as `uploaded_after`.

* `file_type_id` - filters the files returned to only those that have the specified file type/metadata template.

_**Note:** The above filter parameters can be combined_

Example urls (url encoded):

* `GET /folders/<folder_id>/files.json?uploaded_after=2014-06-10%2001:00:00%20GML%2B00:00`

* `GET /folders/<folder_id>/files.json?updated_after=2014-06-10`

* `GET /folders/<folder_id>/files.json?file_type_id=149`

* `GET /folders/<folder_id>/files.json?file_type_id=149&updated_after=2014-06-10`

Get File
--------

* `GET /files/<file_id>.json` returns the specified file.

```json
{
  "id": "<file_id>",
  "filename":"<filename>",
  "created_at":"2013-05-20T12:58:05Z",
  "updated_on":"2013-05-20T13:03:33Z",
  "size":2974,
  "width":15,
  "height":19,
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
      "value":""
    },
    {
      "term_id":"<term_id2>",
      "value":""
    },
    {
      "term_id":"<term_id3>",
      "value":""
    },
    {
      "term_id":"<term_id4>",
      "value":""
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
}
```

Upload File From URL
-----------

* `POST /files.json` uploads a file from a provided URL.

Required parameters:
* `filename` - Name of the file being uploaded.
* `folder_id` - ID of the folder the file will be uploaded to.
* `file_type_id` - ID of the metadata template/file type that the file will have. These can be acquired with [`GET /file_types.json`](https://github.com/ofelcan164/API/blob/Improve-Docs/sections/file_types.md#get-file-types).
* `terms` - List of metadata terms that the file may have associated with its file type/metadata template. If a given file type has 10 terms associated with it, you may include 0-10 of those terms in your calls and terms you do not specify will have empty values. These terms should be passed in the form of a hash, with a `term_id` and a `value`. `term_id` can be found with [`GET /file_types/<file_type_id>.json`](https://github.com/imagerelay/API/blob/master/sections/file_types.md#get-file-type). If you don't want to set any of the metadata on upload, `terms` can be nil.
* `url` - URL of where the file will be downloaded from. Then, it is uploaded to your library in the specified folder.

* `keyword_ids` are optional and should be an array if included. To get keywords/tags see [Keywords Sets/Keywords](https://github.com/imagerelay/api/blob/master/sections/keywording.md).

```json
{
  "filename":"<filename>",
  "folder_id":"<folder_id>",
  "file_type_id":"<file_type_id>",
  "terms": [
    {
      "term_id": "<term_id>",
      "value": "hello world"
    }
  ],
  "keyword_ids": ["<keyword_id1>"],
  "url":"<url_to_download_from>"
}
```
We will return a JSON representation of the uploaded file and `201 OK` if the file was uploaded successfully.

_**Note:** Keywords and tags are the same thing in the request and response bodies but the request body in your call must match exactly how it appears above._

```json
{
  "id": "<uploaded_file_id>",
  "filename": "test.jpg",
  "created_at": "2015-06-03T12:58:43Z",
  "updated_on": "2015-06-03T12:58:43Z",
  "size": 0,
  "width": null,
  "height": null,
  "content_type": "image/jpeg",
  "user_id": "<user_id>",
  "deleted": null,
  "deletion_date": null,
  "delete_user_id": null,
  "expires_on": null,
  "expiring_user_id": null,
  "file_type_id": "<file_type_id>",
  "short_lived_thumbnail": "/static/processing.png",
  "terms": [
    {
      "term_id": "<term_id1>",
      "value": "hello world"
    },
    {
      "term_id": "<term_id2>",
      "value": ""
    },
    {
      "term_id": "<term_id3>",
      "value": ""
    },
    {
      "term_id": "<term_id4>",
      "value": ""
    },
    {
      "term_id": "<term_id5>",
      "value": ""
    },
    {
      "term_id": "<term_id6>",
      "value": ""
    }
  ],
  "tags": [
    {
      "tag_id": "<tag_id>",
      "tag_value": "A Tag"
    }
  ]
}
```

Update File Metadata Terms
-------------

* `POST /files/<file_id>/terms` will update the metadata terms of the file specified.

Required parameters:
  * `terms` - an array of term ids and values to update.
  * `overwrite` - if true, it will overwrite the entire value for that term, if false, it will append the value to any existing metadata term value already present in that term field.

```json
{
  "terms": [
    {
      "term_id": "<term_id1>",
      "value": "The quick brown fox"
    },
    {
      "term_id": "<term_id2>",
      "value": "All rights reserved"

    }
  ],
  "overwrite": true
}
```

Update File Tags
-------------

* `POST /files/<file_id>/tags` will update the tags/keywords of the file specified.

Required parameters:
  * `tags` - contains a list of tag ids to both add and remove from the asset.

```json
{
  "tags":
    {
      "add_ids": [ "<tag_id1>", "<tag_id2>" ],
      "remove_ids": [ "<tag_id3>", "<tag_id4>" ]
    }
}
```

Delete File
-------------

* `DELETE /files/<file_id>` will delete the specified file.

Returns `204 No Content` if successful


Move File
-------------

* `POST /files/<file_id>/move` will move the specified file to the specified folders, removing it from any others the file was previously in.

```json
{
  "folder_ids": [ "<folder_id1>", "<folder_id2>" ]
}
```

Synced File / Copy
-------------
Synced Files let you have the exact same file in multiple folders. When you make a change to any Synced File, that change applies to all instances of the Synced File.

* `POST /files/<file_id>/synced_file` or `POST /files/<file_id>/copy` will create a synced file within the specified folder ids, leaving it in any existing folders.

 The JSON body should contain an array of folder ids

```json
{
  "folder_ids": [ "<folder_id1>", "<folder_id2>" ]
}
```

_**Note:** We will be removing the `/files/<file_id>/copy` endpoint soon - please start using `/synced_file` in your integrations._


Duplicate File
-------------

* `POST /files/<file_id>/duplicate` will create a duplicate of the file in a single destination folder. This duplicate behaves as a completely new file and is completely separate from the original file.

 You can choose to copy all metadata, tags/keywords, and IPTC fields or not. If you choose to not copy metadata (`"should_copy_metadata": false`) - the copied file will get the destination folder's default file type.
 Here's an example JSON body:

 ```json
 {
	"folder_id": "<folder_id>",
	"should_copy_metadata": false
 }
 ```
 _**Note:** There can only be a single destination `folder_id`, not an array of ids_

Asset Thumbnail
-------------

* `POST /files/<file_id>/thumbnail?file_name=image.png` will update the asset thumbnail.

Make sure that the header `Content-Type: application/octet-stream` is present and the request body is the binary data of the new thumbnail file. The total file size must be 5Mb or less. Only JPG and PNG image types are supported.

A `file_name` parameter is required in the query string. Make sure the `file_name` includes an extension otherwise you will receive an error.

Here is an example curl request that illustrates how to use the API.

```shell
curl -u "user:pass" \
  -X "POST" \
  -H "User-Agent: MyApp (you@example.com)" \
  -H "Content-Type: application/octet-stream" \
  --data-binary "@image.png" \
 https://api.imagerelay.com/api/v2/files/file_id/thumbnail?file_name=image.png
```


(DEPRECATED) Update Version
---------------

This endpoint has been deprecated. Use the "Chunked Update Versions" endpoint instead.

* `POST /files/<file_id>/version` will update the underlying file for the file specified with the id (in example id=555).
 The request body should be the binary data of the new file. Make sure to set the Content-Length header. The Content-Type header should be application/octet-stream, the maximum file size supported for this update is 1 GB.

 With `curl`, here is an example:

```shell
curl --data-binary @myfile.jpg \
       -u user:pass \
       -H 'Content-Type: application/octet-stream' \
       -H 'User-Agent: IR (example@imagerelay.com)' \
       https://api.imagerelay.com/api/v2/files/file_id/version.json
```


Chunked Update Versions
---------------

To update an asset you will need to upload your new file in chunks. Each chunk must be under 5Mb.

File chunks are grouped together using a v4 UUID.

### Obtaining a v4 UUID

`POST /api/v2/files/<file_id>/versions` will return a JSON response with a valid v4 UUID to be used when uploading the new file in chunks.

_**Note:** <file_id> is the ID of the file that will be updated by the new, chunk uploaded file._

```json
{
  "uuid": "<valid_v4_uuid>"
}
```

_**Note:** "versions" is pluralized in the path_

### Uploading Chunks
First, split your file into chunks. Chunk size is up to you but must be 5 MB or less. If you attempt to upload a chunk larger than 5 MB you'll receive an error.
Then:

* `POST /api/v2/files/<file_id>/versions/<v4_uuid>/chunk/<chunk_number>` to upload a chunk.

The last number is the chunk number. This is used,
to determine the order to reassemble the chunks on the server. So the second chunk would be `POST /api/v2/files/<file_id>/versions/<v4_uuid>/chunk/2`.
The request body should be the binary data of the chunk. Make sure to set the Content-Length header. The Content-Type header should be `application/octet-stream`

You will receive a `204 No Content`, http status code when the chunk has uploaded successfully.

### Completing the Upload

After all chunks have uploaded successfully you will have to tell the API that the file is ready for processing.

* `POST /api/v2/files/<file_id>/versions/<v4_uuid>/complete` will queue your file for processing.

This endpoint requires a JSON object in the request body with the Content-Type set to `application/json`.

The JSON body you post must contain two attributes, a `file_name` and `chunk_count`.

```json
{
  "file_name": "<filename>",
  "chunk_count": "<number_of_chunks>"
}
```

The `file_name` used here will replace the name of the asset. Also make sure that the `file_name` has an extension.

You will receive a `201 Created` http status code on success.

### Example

Here is an example using `curl` to create a new version of an asset.

**Step 1:** Request a valid v4 UUID from the API

```
curl -u "user:pass" \
  -X POST \
  -H 'User-Agent: MyApp (you@example.com)' \
  "https://api.imagerelay.com/api/v2/files/555/versions"
```

**Step 2:** Start uploading chunks.

 _**Note:** The chunk number at the end of the path corresponds to the chunk being uploaded_

```
# chunk 1
curl -u "user:pass" \
  -X POST \
  -H 'Content-Type: application/octet-stream'  \
  --data-binary "@chunk1" \
  "https://api.imagerelay.com/api/v2/files/555/versions/abc-def-ghi/chunk/1"

# chunk 2
curl -u "user:pass" \
  -X POST \
  -H 'Content-Type: application/octet-stream' \
  --data-binary "@chunk2" \
  "https://api.imagerelay.com/api/v2/files/555/versions/abc-def-ghi/chunk/2"
```

**Step 3:** After all chunks have uploaded you may now make a call to the `complete` endpoint to start processing the file.

```
curl -u "user:pass" \
  -X POST \
  -H 'Content-Type: application/json' \
  -d '{"file_name":"image.png", "chunk_count":2}' \
  "https://api.imagerelay.com/api/v2/files/555/versions/abc-def-ghi/complete"

```

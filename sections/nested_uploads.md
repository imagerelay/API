
Nested File Uploads
=======

The nested upload API is how you can add nested files to Image Relay. Nested files are uploaded to Image Relay through the creation of upload jobs, then multiple files are sent up in chunks. This may be desired, for example, for an image in multiple formats.

Create Upload Job
-----------------

* `POST /upload_jobs.json` will start an upload job.

You'll need the ID from this to upload files. Each upload job will
create *one* asset. Each file specified within the `files` array will be grouped as a part of this asset.
If a jpg is supplied as one of the nested files, that will always be used to create the thumbnail preview for the asset.
If no jpg is provided a preview will still be generated from the file if it is possible.
Include a value for `prefix` if you would like your uploaded, nested asset to be placed in a new folder created as a child of the specified folder. `name` and `size` for each file to be uploaded to the nested asset is required. `name` must include an file extension and `size` is in bytes.

```json
{
    "folder_id":"<folder_id>",
    "file_type_id":"<file_type_id>",
    "prefix":"<new_folder_name>"
    "files":
        [   {
                "name":"<filename1>",
                "size":"1234567890"
            },
            {
                "name":"<filename2>",
                "size":"12231796"
            }
        ]
    "terms":
        [
            {
                "term_id":"<term_id1>",
                "value":"Copyright 2013"
            },
            {
                "term_id":"<term_id2>",
                "value":"sunset over Lake Champlain"
            }
        ]
}
```

This will return `201 Created` if successful, as well as a json representation of the upload job:

```json
{
    "id":"<upload_id>",
    "created_at":"2013-02-07T21:07:36Z",
    "files":
        [
            {
                "id":"<asset_id1>",
                "name":"<filename1>",
                "size":1234567890
            },
            {
                "id":"<asset_id2>",
                "name":"<filename2>",
                "size":12231976
            }
        ]
}
```

Create a File Chunk
-------------------

* `POST /upload_jobs/<upload_id>/files/<asset_id>/chunks/1.json` uploads a file chunk. The last number is the chunk number. This is used,
to determine the order to reassemble the chunks on the server. So the next chunk would be `POST /upload_jobs/<upload_id>/files/<asset_id>/chunks/2.json`.
The request body should be the binary data of the chunk. Make sure to set the Content-Length header. The Content-Type header should be `application/octet-stream`. Remember to change the `<asset_id>` when uploading chunks for different files included in the nested asset.

Chunk size is up to you, up to 5 MB in size. If you attempt to upload a chunk larger than 5 MB you'll receive an error.

POST requests to the chunk endpoint will return the following response codes:

* `201`: File upload chunk processed successfully and the file upload is complete.
* `204`: File upload chunk processed successfully, waiting for more file chunks for the complete file upload.
* `422`: File upload chunk failed, please ensure your file chunk is 5MB or less and retry.

Once all files for an upload job have been uploaded, an asset will be created automatically on Image Relay.

With `curl`, here is an example:

```shell
curl --data-binary @chunk.bin \
       -u user:pass \
       -H 'Content-Type: application/octet-stream' \
       -H 'Content-Length: [chunk size in bytes]' \
       -H 'User-Agent: IR (example@imagerelay.com)' \
       https://api.imagerelay.com/api/v2/upload_jobs/384/files/395/chunks/1.json
```


# Additional Asset Meta Data

You may include the optional `expires_on` and `keyword_ids` attributes in the JSON payload when posting to the `/api/v2/upload_jobs.json` endpoint.

* `expires_on` - must be a valid date time, otherwise posting chunks will fail. This attribute is used for setting the expiration date of an asset.

* `keyword_ids` - must contain an array of valid keyword ids from your account, otherwise posting chunks will fail. This attribute is used for attaching existing keywords to an asset.

_**Note:** You do not need to include the keyword set id, just the id of the keyword._

You may create keywords from the API, however, you must have the correct permissions to do so. You may retrieve keyword ids from the API as well. See the [Keywording](https://github.com/imagerelay/API/blob/master/sections/keywording.md) for more information.

Here is an example curl request that uses the above metadata attributes to create an upload job.

```json
curl -XPOST -u "yourUsername:yourPassword" "https://api.imagerelay.com/api/v2/upload_jobs.json" \
  -H 'Content-Type: application/json' \
  -H 'User-Agent: MyApp (yourName@example.com)' \
  -d '{ "expires_on": "September 31, 2020", "keyword_ids": ["<keyword_id1", "<keyword_id2", "<keyword_id3"], "prefix": "", "folder_id":"<folder_id>","file_type_id":"<file_type_id>", "terms":[], "files": [ {"name": "<filename1>", "size": "7387" }, {"name": "<filename2>", "size": "116532" } ] }
```

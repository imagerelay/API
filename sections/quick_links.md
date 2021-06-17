Quick Links
===========

Quick links allow you to quickly share a single file with someone else. They also can be embedded in HTML allowing you to
server your Image Relay files from your own sites directly from Image Relay. Downloads via quick links are counted against
your company's bandwidth account limits.

Get Quick Links
---------------

* `GET /quick_links.json` will return `200 OK` and the authenticated user's list of quick links

We will return 100 quick links per page. If the result set has 100 quick links, it's your responsibility to check the next page to see if there are any more quick links -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id": "<quick_link_id1>",
    "uid": "<uid>",
    "purpose": null,
    "created_at": "2013-08-12T14:58:34Z",
    "processing": false,
    "asset_id": "<asset_id1>",
    "user_id": "<user_id>",
    "url": "<quick_link_url>"
  },
  {
    "id": "<quick_link_id2>",
    "uid": "<uid>",
    "purpose": null,
    "created_at": "2013-08-12T14:58:58Z",
    "processing": false,
    "asset_id": "<asset_id2>",
    "user_id": "<user_id>",
    "url": "<quick_link_url>"
  },
  {
    "id": "<quick_link_id3>",
    "uid": "<uid>",
    "purpose": "API",
    "created_at": "2012-05-04T14:04:17Z",
    "processing": false,
    "asset_id": "<asset_id3>",
    "user_id": "<user_id>",
    "url": "<quick_link_url>"
  }
]
```

Get Quick Link
--------------

* `GET /quick_links/<quick_link_id>.json` will return `200 OK` and the specified quick link.

```json
{
    "id": "<quick_link_id>",
    "uid": "<uid>",
    "purpose": "API",
    "created_at": "2012-11-30T09:21:54Z",
    "processing": false,
    "asset_id": "<asset_id>",
    "user_id": "<user_id>",
    "url": "<quick_link_url>"
}
```

Create Quick Link
-----------------
Quick links can be used to generate a URL to download/embed an asset, or to download/embed a converted (and or resized) version of the asset in jpg or png format.

* `POST /quick_links.json` will create a new quick link.

Required parameters are `purpose` and `asset_id`. All other parameters are optional.

If you are using the quick link URL to download the asset then do not include the `disposition` parameter in the post body.

If you are using the quick link to render the image inline in a web browser or using it in an `<img>` tag, for example, then include `"disposition": "inline"` in the post body.

If you want to downsize or convert the asset to a jpg or png then include the `max_width` and `max_height` parameters in addition to the `format` parameter in the post body. Accepted `formats` are `jpg` and `png`.

_**Note:** We can only downsize images. If you post dimensions that are larger than the asset's dimensions - the quick link will be sized to the asset's dimensions._

```json
{
  "purpose": "Embed in marketing web site",
  "asset_id": "<asset_id>",
  "expires": "2013-08-12",
  "max_width": "200",
  "max_height": "300",
  "format": "jpg",
  "dpi": 72,
  "disposition": "inline",
  "color_format": "rgb"
}
```

This endpoint returns `201 Created`, if successful, as well as a representation of the quick link.

```json
{
    "id": "<quick_link_id>",
    "uid": "<uid>",
    "purpose": "API",
    "created_at": "2012-11-30T09:21:54Z",
    "processing": true,
    "asset_id": "<asset_id>",
    "user_id": "<user_id>",
    "url": "<quick_link_url>"
}
```

If the quick link `processing` flag is `true`, then the image hasn't finished resizing or converting yet. It will not be ready to use until "processing" is false. You can poll every few seconds (we recommend every 5 seconds), to see if the quick link is ready for viewing yet.

Delete Quick Link
-----------------

* `DELETE /quick_links/<quick_link_id>.json` will delete the specified quick link.

This will return `204 No Content`, if successful.

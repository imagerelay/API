File Types
==========

Each file in Image Relay is assigned a file type. Different file types have different sets of metadata terms associated with them.

_**Note:** On the web, file types are referred to interchangeably with Metadata Templates. Not all plans have the ability to create new templates and alter the terms associated with your default template. Accounts require `Admin Access to Metadata Tools` for this functionality to be accessible. Please contact support@imagerelay.com if you have questions about your plan features._

Get File Types
--------------

* `GET /file_types.json` will return all the file types for the account given that the authenticated user is within a permission group with metadata access.

```json
[
  {
    "name":"<file_type_name1>",
    "description":"",
    "created_on":"2008-05-18T20:16:52Z",
    "updated_on":"2013-01-07T17:06:14Z",
    "id":"<file_type_id>",
    "terms":
      [
        {
          "name":"<metaterm_name1>",
          "position":1,
          "meta_name":"",
          "display_in_download":null,
          "field_type": "text_field",
          "metaterm_options": [],
          "created_on":"2012-06-08T12:44:38Z",
          "updated_on":"2012-06-08T12:44:38Z",
          "id":"<term_id>"
         },
         {
           "name":"<metaterm_name2>",
           "position":2,
           "meta_name":"",
           "display_in_download":null,
           "field_type": "text_field",
           "metaterm_options": [],
           "created_on":"2012-06-08T12:44:38Z",
           "updated_on":"2012-06-08T12:44:38Z",
           "id":"<term_id>"
         },
      ]
  },
  {
    "name":"<file_type_name2>",
    "description":"",
    "created_on":"2009-05-06T10:48:00Z",
    "updated_on":"2009-05-06T12:43:30Z",
    "id":"<file_type_id>",
    "terms":
      [
        {
          "name":"<metaterm_name>",
          "position":1,
          "meta_name":"Title",
          "display_in_download":null,
          "field_type": "text_field",
          "metaterm_options": [],
          "created_on":"2009-05-06T10:48:00Z",
          "updated_on":"2009-05-06T10:48:00Z",
          "id":"<term_id>"
        }
      ]
  }
]
```

Get File Type
-------------

* `GET /file_types/<file_type_id>.json` will return the specified file type.

```json
{
  "name":"<file_type_name1>",
  "description":"",
  "created_on":"2008-05-18T20:16:52Z",
  "updated_on":"2013-01-07T17:06:14Z",
  "id":"<file_type_id>",
  "terms":
    [
      {
        "name":"<metaterm_name1>",
        "position":1,
        "meta_name":"",
        "display_in_download":null,
        "field_type": "text_field",
        "metaterm_options": [],
        "created_on":"2012-06-08T12:44:38Z",
        "updated_on":"2012-06-08T12:44:38Z",
        "id":"<term_id>"
       },
       {
         "name":"<metaterm_name2>",
         "position":2,
         "meta_name":"",
         "display_in_download":null,
         "field_type": "text_field",
         "metaterm_options": [],
         "created_on":"2012-06-08T12:44:38Z",
         "updated_on":"2012-06-08T12:44:38Z",
         "id":"<term_id>"
       },
    ]
}
```

Create File Type
-------------

_**COMING SOON**_

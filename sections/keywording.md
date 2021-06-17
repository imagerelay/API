Keywording
============

Keywording allows you to add searchable keywords to your assets.

_**Note:** Keywords and tags are the same thing and are interchangeable throughout all our documentation._


Keyword/Tag Sets
============
Keyword sets are groups of keywords that can be associated with an asset

Get Keyword Sets
----------------
* `GET /keyword_sets.json` will return `200 OK` and the authenticated user's list of keyword sets given their permissions group allows for metadata access.

We will return 100 keyword sets per page. If the result set has 100 keyword sets, it's your responsibility to check the next page to see if there are any more keyword sets -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id": "<keyword_set_id1>",
    "name": "<keyword_set_name>",
    "created_at": "2015-06-15T11:30:21Z",
    "updated_at": "2015-06-15T11:30:21Z",
    "deleted_at": null
  },
  {
    "id": "<keyword_set_id2>",
    "name": "<keyword_set_name>",
    "created_at": "2015-06-12T18:21:43Z",
    "updated_at": "2015-06-12T18:22:07Z",
    "deleted_at": null
  }
]
```

Get Keyword Set
---------------
* `GET /keyword_sets/<keyword_set_id>.json` will return `200 OK` and the specified keyword set.

```json
{
  "id": "<keyword_set_id>",
  "name": "<keyword_set_name>",
  "created_at": "2015-06-15T11:30:21Z",
  "updated_at": "2015-06-15T11:30:21Z",
  "deleted_at": null
}
```

Create Keyword Set
------------------
* `POST /keyword_sets.json` will create a new keyword set. When a keyword set is created, it will be initially empty. Then, individual keywords can be created and added to the keyword set. See [Create Keyword](https://github.com/ofelcan164/API/blob/Improve-Docs/sections/keywording.md#create-keyword).

```json
{
	"name":"<new_keyword_set_name>"
}
```

We will return `201 Created` and a JSON representation of the new keyword set.


```json
{
  "id": "<new_keyword_set_id>",
  "name": "<new_keyword_set_name>",
  "created_at": "2015-06-16T08:52:12Z",
  "updated_at": "2015-06-16T08:52:12Z",
  "deleted_at": null
}
```

Update Keyword Set
------------------
* `PUT /keyword_sets/<keyword_set_id>.json` will update the `name` of the specified keyword set.
```json
{
	"name":"<new_keyword_set_name>"
}
```
We will return `200 OK` and a JSON representation of the specified keyword set.

```json
{
  "id": "<keyword_set_id>",
  "name": "<new_keyword_set_name>",
  "created_at": "2015-06-16T08:52:12Z",
  "updated_at": "2015-06-16T08:52:12Z",
  "deleted_at": null
}
```

Delete Keyword Set
-------------------
* `DELETE /keyword_sets/<keyword_set_id>.json` will delete the specified keyword set and return `204 No Content` upon success. All of the keyword set's associated keywords/tags will also be deleted.

_**Note:** If identical keywords exists in multiple keyword sets, only the keywords in the deleted set will be deleted._

Keywords/Tags
========

Keywords are associated with keyword sets and are searchable.

Get Keywords
------------
* `GET /keyword_sets/<keyword_set_id>/keywords.json` will return `200 OK` and a list of keywords/tags associated with the specified keyword set.

```json
[
  {
    "id": "<keyword_id1>",
    "keyword_set_id": "<keyword_set_id>",
    "name": "<keyword1>",
    "created_at": "2015-06-15T12:34:36Z",
    "updated_at": "2015-06-15T14:05:34Z"
  },
  {
    "id": "<keyword_id2>",
    "keyword_set_id": "<keyword_set_id>",
    "name":"<keyword2>",
    "created_at": "2015-06-16T08:59:48Z",
    "updated_at": "2015-06-16T08:59:48Z"
  },
]
```

Get Keyword
-----------
* `GET /keyword_sets/<keyword_set_id>/keywords/<keyword_id>.json` will return `200 OK` and a representation of the specified keyword requested.

```json
{
	"id": "<keyword_id>",
	"keyword_set_id": "<keyword_set_id>",
	"name": "<keyword>",
	"created_at": "2015-06-15T12:34:36Z",
	"updated_at": "2015-06-15T14:05:34Z"
}
```

Create Keyword
--------------
* `POST /keyword_sets/<keyword_set_id>/keywords.json` will create a new keyword and associate it with the specified keyword set.

```json
{
	"name":"<new_keyword>"
}
```

We will return `201 Created` and a JSON representation of the new keyword

```json
{
  "id": "<keyword_id>",
  "keyword_set_id": "<keyword_set_id>",
  "name": "<new_keyword>",
  "created_at": "2015-06-16T09:07:30Z",
  "updated_at": "2015-06-16T09:07:30Z"
}
```

Update Keyword
--------------
* `PUT /keyword_sets/<keyword_set_id>/keywords/<keyword_id>.json` will update the specified keyword.

```json
{
	"name":"<updated_keyword>"
}
```

We will return `200 OK` and a JSON representation of the updated keyword.

```json
{
  "id": "<keyword_id>",
  "keyword_set_id": "<keyword_set_id>",
  "name": "<updated_keyword>",
  "created_at": "2015-06-16T09:07:30Z",
  "updated_at": "2015-06-16T09:07:30Z"
}
```

Delete Keyword
--------------
* `DELETE /keyword_sets/<keyword_set_id>/keywords/<keyword_id>.json` will delete the specified keyword and return `204 No Content`.

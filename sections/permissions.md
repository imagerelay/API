Permissions
==========

Allow you to retrieve information about different permissions groups.

Get Permissions
--------------

* `GET /permissions.json` will return `200 OK` and a JSON representation of all the permission groups for the authenticated user's account.

We will return 100 permissions per page. If the result set has 100 permissions, it's your responsibility to check the next page to see if there are any more permissions -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

```json
[
  {
    "id":"<permission_group_id1>",
    "name":"<permission_group_name1>",
    "description":"admin",
    "created_on":null,
    "updated_on":"2015-05-20T19:47:30Z"
  },
  {
    "id":"<permission_group_id2>",
    "name":"<permission_group_name2>",
    "description":"testing asset permissions",
    "created_on":"2009-05-05T08:53:37Z",
    "updated_on":"2011-04-20T10:59:20Z"
  }
]
```

Get Permission
-------------

* `GET /permissions/<permission_group_id>.json` will return `200 OK` and a JSON representation of the specified permission group.

```json
{
  "id":"<permission_group_id>",
  "name":"<permission_group_name>",
  "description":"admin",
  "created_on":null,
  "updated_on":"2015-05-20T19:47:30Z"
}
```

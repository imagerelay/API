Invited Users
===========

Allows you to retrieve information about users that have been invited to your Image Relay account, invite a new user, or delete a invited user.

Get Invited Users
---------------

* `GET /invited_users.json` will return a list of users who have been invited to your account but have yet to complete the full registration process. The invited users will be ordered by the most recently created.

We will return 100 invited users per page. If the result set has more than 100 invited users, it's your responsibility to check the next page to see if there are any more invited users -- you do this by adding `&page=2` to the query, then `&page=3` and so on.

_**Note:** <invited_user_id> is distinct from <inviting_user_id>._


```json
[
	{
		"id": "<invited_user_id>",
		"user_id": "<inviting_user_id>",
		"sub_domain_id": "sub_domain_id",
		"first_name": "First Name",
		"last_name": "Last Name",
		"email": "example@imagerelay.com",
		"company": "Company",
		"custom_field_one": null,
		"custom_field_two": null,
		"custom_field_three": null,
		"custom_field_four": null,
		"created_at": "2021-06-14T15:12:29.000Z",
		"updated_at": "2021-06-14T15:12:29.000Z",
		"permission_id": "<permission_group_id>",
		"registration_url": "<registration_url>"
	},
	{
		...
	}
]
```

* `GET /invited_users/<invited_user_id>.json` will return the specified invited user.

```json
{
	"id": "<invited_user_id>",
	"user_id": "<inviting_user_id>",
	"sub_domain_id": "sub_domain_id",
	"first_name": "First Name",
	"last_name": "Last Name",
	"email": "example@imagerelay.com",
	"company": "Company",
	"custom_field_one": null,
	"custom_field_two": null,
	"custom_field_three": null,
	"custom_field_four": null,
	"created_at": "2021-06-14T15:12:29.000Z",
	"updated_at": "2021-06-14T15:12:29.000Z",
	"permission_id": "<permission_group_id>",
	"registration_url": "<registration_url>"
}
```

Invite New User
---------------

* `POST /invited_users.json` will create a new invited user.

The Content-Type header should be `application/json`

```json
{
	"first_name":"First Name",
	"last_name":"Last Name",
	"email":"example@imagerelay.com",
	"company":"Image Relay",
	"permission_id":"<permission_group_id>"
}
```

We will return a response with JSON data on the new invited user and the user will be sent an invite email to signup for Image Relay.

```json
{
	"id": "<invited_user_id>",
	"user_id": "<inviting_user_id>",
	"sub_domain_id": "<sub_domain_id>",
	"first_name": "First Name",
	"last_name": "Last Name",
	"email": "example@imagerelay.com",
	"company": "Company",
	"custom_field_one": null,
	"custom_field_two": null,
	"custom_field_three": null,
	"custom_field_four": null,
	"created_at": "2021-06-14T15:12:29.000Z",
	"updated_at": "2021-06-14T15:12:29.000Z",
	"permission_id": "<permission_group_id>",
	"registration_url": "<registration_url>"
}
```

If the invite is successful, the call will return `201 Created`. Otherwise you will receive `400 Bad Request`.

Delete Invited User
--------------------

* `DELETE /invited_users/<invited_user_id>.json` will delete the specified invited user. This is only possible via the API if the invited user has yet to complete the registration process and is thus present in the `Inivted Users` table on your Image Relay account on the web.

This will return `204 No Content`, if successful.

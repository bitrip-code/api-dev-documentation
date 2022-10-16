# Routes

BaseRoute: `https://api.bitrip.com`

Security:

Must pass api key in headers behind `x-api-key`

Example:

```js
{
  "x-api-key": "your-api-key-goes-here",
  "Content-Type": "application/json"
}
```

Routes:

- <a href="#get_org">`GET: /org`</a> - returns `Map`
- <a href="#get_user">`GET: /user`</a> - Accepts query string, returns `Map`
- <a href="#get_label_by_id">`GET: /label/{labelId}`</a> - Accepts label id parameter, returns `Map`
- <a href="#post_label">`POST: /label`</a> - Accepts JSON uid, tags, name, and pid body data, returns `Map`
- <a href="#put_label">`PUT: /label/{labelId}`</a> - Accepts label id parameter, returns `Map`
- <a href="#delete_label">`DELETE: /label/{labelId}`</a> - Accepts label id parameter, returns `Map`
- <a href="#delete_label_project">`DELETE: /label/from_project/{labelId}`</a> - Accepts label id parameter, returns `Map`
- <a href="#get_labels">`GET: /labels`</a> - Accepts query string, returns `Array`
- <a href="get_project_by_id">`GET: /project/{projectId}`</a> - Accepts project id parameter, returns `Map`
- <a href="#get_projects">`GET: /projects` </a> - Accepts query string, returns `Array`
- <a href="#get_logs">`GET: /logs`</a> - Accepts query string, returns `Array`

<br>

- Success

  - Successful requests will receive a status code 200 and JSON payload.

- ! Error
  - Errors are handled with a status 500 code and a `msg` property with error details attached.

<div id="get_org">

## `GET: /org`

Gets org info including member and admin ids for further querying.

Example request:

`https://api.bitrip.com/org`

Example Response:

```json
{
  "admins": [
    "KFzra4FuAjbQCMawNQ0AlJoGmqv1", //user IDS
    "dn82sDBSlbNj4hY9Hx0m490hIEl2"
  ],
  "subscribers": [],
  "members": [],
  "invitedAdmins": [],
  "created": {
    "_seconds": 1664645679,
    "_nanoseconds": 486000000
  },
  "name": "Oakland",
  "id": "f11b4f25-4dbe-44a8-a723-bc612eeda4b9",
  "creatorId": "KFzra4FuAjbQCMawNQ0AlJoGmqv1",
  "invitedMembers": []
}
```

</div>

<br>

<div id="get_user">

## `GET: /user`

Gets a user by their ID.

Example Request:

`https://api.bitrip.com/user?uid=dn82sDBSlbNj4hY9Hx0m490hIEl2`

Example Response:

```json
{
  "displayName": "Thomas - test",
  "email": "thomas.smith.br.testing@gmail.com",
  "uid": "dn82sDBSlbNj4hY9Hx0m490hIEl2",
  "organisationId": "f11b4f25-4dbe-44a8-a723-bc612eeda4b9",
  "projects": [
    "b549ad06-eb5a-4fa6-88dc-4d80bbc2beae", //Project IDS
    "480aea87-df2e-489d-9829-4daa35a13c02",
    "aae48908-34fc-4d7b-80aa-8d7e4864325a",
    "418b2ae1-a237-46f3-a6ac-4145e7f74aee"
  ]
}
```

</div>

<br>

<div id="get_label_by_id">

## `GET: /label/{labelID}`

Get a single label using a label id after `/label/`

Example Request:

`https://api.bitrip.com/label/7901dd0d-9a0a-4712-81de-169d572f1491`

Example Response:

```json
{
  "tags": ["done", "urgent"],
  "creator_ref": "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
  "projectID": "aae48908-34fc-4d7b-80aa-8d7e4864325a",
  "id": "7901dd0d-9a0a-4712-81de-169d572f1491",
  "modified": {
    "_seconds": 1665084165,
    "_nanoseconds": 107000000
  },
  "created": {
    "_seconds": 1665084149,
    "_nanoseconds": 719000000
  },
  "name": "A label for tags"
}
```

</div>

<div id="post_label">

## `POST /label`

Data must be sent as JSON body information.

- uid (required) User ID
  - this will set the creator_ref for the label
- title (required)
  - sets title for label
- pid (optional)
  - creates label inside of project
- tags (optional)
  - adds searchable keywords to label

Example Request:

```js
fetch("https://api.bitrip.com/label", {
  method: "POST",
  headers: {
    "x-api-key": "your api key here",
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    uid: "user id",
    name: "new label",
    pid: "new project id",
    tags: ["urgent", "review"],
  }),
})
  .then((res) => res.json())
  .then((labelData) => console.log(labelData))
  .catch((err) => {
    console.log(err);
    console.log(err.response.data.msg);
  });
```

</div>

<br>

<div id="put_label">

## `PUT: /label/{labelID}`

Edit labels by sending the updated value.

Update label parameters by sending

- tags - `Array`
- name - `String`
- pid - `String`

Example request:

- updates label name, tags

```js
fetch("https://api.bitrip.com/label", {
  method: "PUT",
  headers: {
    "x-api-key": "your api key here",
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    name: "new label title",
    tags: ["done"],
  }),
})
  .then((res) => res.json())
  .then((labelData) => console.log(labelData))
  .catch((err) => {
    console.log(err);
    console.log(err.response.data.msg);
  });
```

- updates label project

```js
fetch("https://api.bitrip.com/label/41e92e47-133d-4b6a-89d7-a38404ec6024", {
  method: "PUT",
  headers: {
    "x-api-key": "your api key here",
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    pid: "new project id",
  }),
})
  .then((res) => res.json())
  .then((labelData) => console.log(labelData))
  .catch((err) => {
    console.log(err);
    console.log(err.response.data.msg);
  });
```

Example response:

```json
{
  "tags": ["done"],
  "creator_ref": "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
  "id": "41e92e47-133d-4b6a-89d7-a38404ec6024",
  "modified": {
    "_seconds": 1665693479,
    "_nanoseconds": 976000000
  },
  "created": {
    "_seconds": 1665692096,
    "_nanoseconds": 189000000
  },
  "name": "new label title"
}
```

</div>

<br>

<div id="delete_label">

## `DELETE: /label/{labelID}`

Delete a single label using a label id after `/label/`.

Example Request:

`https://api.bitrip.com/label/946b825f-af94-49c1-8c33-409ef44c758b`

Example Response:

```json
{
  "msg": "Label deleted successfully."
}
```

</div>
<br>

<div id="delete_label_project">

## `DELETE: /label/{labelID}`

Removes label from project using a label id after `/label/`.

Delete a single label using a label id after `/label/`.

Example Request:

`https://api.bitrip.com/label/from_project/946b825f-af94-49c1-8c33-409ef44c758b`

Example Response:

```json
{
  "msg": "Label removed from project successfully."
}
```

</div>

<br>

<div id="get_labels">

## `GET: /labels`

Optional query parameters:

- `lid` (optional) label ID.
  - Gets label by its ID; Used standalone. **_Doesn't work with other
    query params._** Returns single object.
- `uid` (optional) user ID
  - Used to get labels by org member id.
- `pid` (optional) project ID
  - Gets labels by their project ID.
- `has_o` (optional) comma separated values
  - Get labels by the tags attached to them
- `from` (optional) EPOCH timestamp
  - Defaults to all but can be used with type to narrow search.
- `type` (optional) scan type (edited, viewed, scanned)
  - Narrows search to type of interaction.
- `when` (optional) EPOCH timestamp.
  - Time stamp to limit search time
- `to` (must be used with when) EPOCH timestamp

While these query params are optional you must pass either a pid, uid,
lid, or has_o

Example end points:

- Gets all labels with the `done` or `urgent` tag from October 5, 2022
  6:00:57 PM

`https://api.bitrip.com/labels?has_o=done,urgent&from=1664992857`

- Get all labels from a project by pid

`https://api.bitrip.com/labels?pid=aae48908-34fc-4d7b-80aa-8d7e4864325a`

- Get all labels by specific user that was viewed since October 5, 2022
  6:00:57 PM and belongs to a project.

`https://api.bitrip.com/labels?uid=dn82sDBSlbNj4hY9Hx0m490hIEl2&pid=aae48908-34fc-4d7b-80aa-8d7e4864325a&type=viewed&from=1664992857`

Example response:

```json
[
    {
        "tags": [
            "done",
            "urgent"
        ],
        "creator_ref": "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
        "projectID": "aae48908-34fc-4d7b-80aa-8d7e4864325a",
        "id": "7901dd0d-9a0a-4712-81de-169d572f1491",
        "modified": {
            "_seconds": 1665084165,
            "_nanoseconds": 107000000
        },
        "created": {
            "_seconds": 1665084149,
            "_nanoseconds": 719000000
        },
        "name": "A label for tags"
    },
    ...
]
```

</div>

<br>

<div id="get_project_by_id">

## `GET: /project/{projectID}`

Get a single project using a project id after `/project/`

Example Request:

`https://api.bitrip.com/project/b549ad06-eb5a-4fa6-88dc-4d80bbc2beae`

Example Response:

```json
{
  "members": [
    "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
    "/users/KFzra4FuAjbQCMawNQ0AlJoGmqv1"
  ],
  "membersCanInvite": false,
  "creator_ref": "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
  "title": "Project from test account",
  "privateLabels": true,
  "admins": [
    "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2",
    "/users/KFzra4FuAjbQCMawNQ0AlJoGmqv1"
  ],
  "id": "b549ad06-eb5a-4fa6-88dc-4d80bbc2beae",
  "modified": {
    "_seconds": 1664645810,
    "_nanoseconds": 177000000
  },
  "contentItems": [],
  "labels": [
    "a5c63c6e-e9f7-492b-893e-e3040e3c15c7",
    "57f1896e-e0f0-458a-bbc2-218d7a557393",
    "a8383f0f-4d74-4a84-b628-eb94432d704f"
  ],
  "created": {
    "_seconds": 1664645810,
    "_nanoseconds": 177000000
  }
}
```

</div>

<br>

<div id="get_projects">

## `GET: /projects`

Optional query parameters:

- `pid` (optional) project ID; Used standalone. **_Doesn't work with other
  query params._** Returns single object.
  - Gets project by the projects ID.
- `uid` (optional) user ID
  - Used to get a project created by org member id.
- `has_o` (optional) comma separated values
  - Get projects by the tags attached to the labels inside them.
- `from` (optional) EPOCH timestamp
  - Defaults to all but can be used with type to narrow search to all
    things after the epoch time stamp.
- `type` (optional) scan type (edited, viewed, scanned)
  - Narrows search to type of interaction with the labels inside of the
    project.
- `when` (optional) EPOCH timestamp.
  - Time stamp to limit search time
- `to` (must be used with when) EPOCH timestamp

If there is no query params sent it will return all the projects in the
organization.

Example requests:

- Get all projects in organization

`https://api.bitrip.com/project`

- Get all projects whose labels have been interacted with since Wednesday,
  October 5, 2022 12:19:32 AM

`https://api.bitrip.com/project?from=1664929172`

- Get all projects that have a label in them with the urgent or done tag

`https://api.bitrip.com/project?from=1664929172&has_o=urgent,done`

- Get all projects that have labels that have been scanned since October
  5, 2022 12:19:32 AM and has urgent or done tags in them.

`https://api.bitrip.com/project?from=1664929172&type=scanned&has_o=urgent,done`

Example Response:

```json
[
    {
        "labels": [
            "b6fcf49e-ba36-4a49-bece-37fb527deed7"
        ],
        "membersCanInvite": false,
        "contentItems": [],
        "modified": {
            "_seconds": 1664892640,
            "_nanoseconds": 756000000
        },
        "title": "done",
        "admins": [
            "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2"
        ],
        "privateLabels": false,
        "members": [
            "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2"
        ],
        "created": {
            "_seconds": 1664892640,
            "_nanoseconds": 756000000
        },
        "id": "418b2ae1-a237-46f3-a6ac-4145e7f74aee",
        "creator_ref": "/users/dn82sDBSlbNj4hY9Hx0m490hIEl2"
    },
    ...
]
```

</div>

<br>

<div id="get_logs">

## `GET: /logs`

Optional query parameters:

- `uid` (optional) user ID
  - Used to get logs by org member id.
- `pid` (optional) get logs by project ID
  - Gets labels by the project ID.
- `from` (optional) EPOCH timestamp
  - Defaults to all but can be used with type to narrow search to all
    things after the epoch time stamp.
- `type` (optional) scan type (edited, viewed, scanned)
  - Narrows search to type of interaction with the label.
- `lid` (optional) label ID
  - Gets label by its ID
- `when` (optional) EPOCH timestamp.
  - Time stamp to limit search time
- `to` (must be used with when) EPOCH timestamp

If there are no query params sent, it will return all logs in the
organization.

Example requests:

Get viewer logs from user since Sat Oct 01 2022

`https://api.bitrip.com/logs?uid=KFzra4FuAjbQCMawNQ0AlJoGmqv1&from=1664665190&type=viewed`

Get view logs from Sat Oct 01 2022 to Thursday, October 6, 2022

`https://api.bitrip.com/logs?from=1664665190&to=1665084152&type=viewed`

Example response:

```json
[
    {
        "loggerName": "Thomas - test",
        "loggedAt": {
            "_seconds": 1664645821,
            "_nanoseconds": 445000000
        },
        "editType": "Assigned \"Project from test account\"",
        "type": "edited",
        "labelId": "a5c63c6e-e9f7-492b-893e-e3040e3c15c7",
        "loggerId": "dn82sDBSlbNj4hY9Hx0m490hIEl2",
        "projectId": "b549ad06-eb5a-4fa6-88dc-4d80bbc2beae"
    },
    ...
]
```

</div>

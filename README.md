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

- `GET: /org` - No Args necessary, returns `Map`
- `GET: /user` - Accepts query string, returns `Map`
- `GET: /label` - Accepts label id parameter, returns `Map`
- `POST: /label` - Accepts JSON uid, tags, and pid body data, returns `Map`
- `GET: /labels` - Accepts query string, returns `Array`
- `GET: /project` - Accepts project id parameter, returns `Map`
- `GET: /projects` - Accepts query string, returns `Array`
- `GET: /logs` - Accepts query string, returns `Array`

<br>

- ! Error
  - Errors are handled with a status 500 code and a `msg` property with error details attached.

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

<br>

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

<br>

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
  headers: {
    "x-api-key": "your api key here",
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    uid: "user id",
    title: "new label",
    pid: "project id",
    tags: ["urgent", "review"],
  }),
})
  .then((res) => res.json())
  .then((labelData) => console.log(labelData))
  .catch((err) => {
    // will display err status code
    console.log(err);
    // will provide further information
    console.log(err.response.data.msg);
  });
```

<br>

## `DELETE: /label/{labelID}`

Delete a single label using a label id after `/label/`.

Example Request:

`http://localhost:5000/label/946b825f-af94-49c1-8c33-409ef44c758b`

Example Response:

```json
{
  "msg": "Label deleted successfully."
}
```

<br>

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

<br>

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

<br>

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

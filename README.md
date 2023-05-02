## Routes

- The base URL for accessing the API is `https://api.bitrip.com`

## Security

- In order to access the API, you must include an API key in the headers of your request using the `x-api-key` field.
- Here is an example of how to include the API key in the headers using JavaScript:

Example:

```js
{
  "x-api-key": "your-api-key-goes-here",
  "Content-Type": "application/json"
}
```

<br/>

## Endpoints:

<a href="#org_get">Get: /org</a>

- returns `Map`

<a href="#user_get_param">GET: /user/{userId}</a>

- Accepts user id param
- returns `Map`

<a href="#user_get_query">GET: /user?email="example@gmail.com"</a>

- Accepts user email query string
- returns `Map`

<a href="#label_get_param">GET: /label/{labelId}</a>

- Accepts label id parameter
- returns `Map`

<a href="#label_post">POST: /label</a>

- Accepts JSON uid, tags, name, and pid body data
- returns `Map`

<a href="#label/attachment_post">POST: /label/attachment/{labelId}</a>

- Accepts label id parameter and JSON title, text, type body data
- returns `Map`

<a href="#label/attachment_put">PUT: /label/attachment/{labelId}</a>

- Accepts labelId as a parameter and an array of attachments sent in the request body.

<a href="#label/attachment_delete">DELETE: /label/attachment/{labelId}</a>

- Accepts labelId as a parameter and an array of attachments sent in the request body.

<a href="#label_put">PUT: /label/{labelId}</a>

- Accepts the following as parameters in the request body: label id, JSON tags, name, uid, pid.
- returns `Map`

<a href="#label_delete">DELETE: /label/{labelId}</a>

- Accepts label id URL parameter.
- returns `Map`

<a href="#label/from_project_delete">DELETE: /label/from_project/{labelId}</a>

- Accepts label id parameter
- returns `Map`

<a href="#labels_get">GET: /labels</a>

- Accepts project id parameter
- returns `Map`

<a href="#projects_get">GET: /projects</a>

- Accepts JSON uid, members, and admins body data
- returns `Map`

<a href="#project_delete">DELETE: /project/{projectId}</a>

- Accepts project id as a URL parameter
- returns success message

<a href="#logs_get">GET: /logs</a>

- Accepts a query string as a parameter
- returns `Array`

<br>

Success

- If the request is successful, the server will respond with a HTTP status code of 200 and a JSON payload in the response body.

‼️ Error

- In case of an error, the server will return a HTTP status code of 500 and an error message in the response body's msg property, containing details about the error.

<br>

<div id="org_get">

## `GET: /org`

- The /org endpoint retrieves information about a Bitrip organization using a GET request.

### Example request:

```js
const url = "https://api.bitrip.com/org";

const options = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key goes here",
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

To successfully call this endpoint, send a GET request to the /org endpoint with the Content-Type header set to application/json. The x-api-key header is required and should be replaced with your API key.
<br />

### Example Response:

```json
{
  "admins": [
    "KFzra4FuAjbQCMawNQ0AlJoGmqv1", // admin user IDS
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

If the request is successful, the API will return a JSON response with the requested organization's information. The response contains the following properties:

- `admins`: An array of user IDs that represent the administrators of the organization.
- `subscribers`: An array of user IDs that represent the subscribers of the organization.
- `members`: An array of user IDs that represent the members of the organization.
- `invitedAdmins`: An array of user IDs that represent the invited administrators of the organization.
- `created`: An object that contains the creation date and time of the organization in seconds and nanoseconds format.
- `name`: The name of the organization.
- `id`: A unique identifier that represents the organization.
- `creatorId`: The ID of the user who created the organization.
- `invitedMembers`: An array of user IDs that represent the invited members of the organization.

Please note that the user IDs included in the response are only provided as examples, and the actual user IDs will be unique strings.

</div>

<br>

<div id="user_get_param">

## `GET: /user/{userID}`

- Get a user by user iD.

### Example Request:

```js
const url = "https://api.bitrip.com/user/dn82sDBSlbNj4hY9Hx0m490hIEl2";

const options = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

### Example Response

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

<br/>

If the request is successful, the API will return a JSON response with the requested user's information. The response contains the following properties:

- `displayName`: The user's display name.
- `email`: The user's email address.
- `uid`: The user ID, which is a unique identifier for the user.
- `organisationId`: The unique identifier of the organization that the user is associated with.
- `projects`: An array of project IDs that represent the projects the user has access to.

Please note that the project IDs included in the response are only provided as examples, and the actual project IDs will be unique strings.

</div>
<br>

<div id="user_get_query">

## `GET /user?email=example@gmail.com`

- Get a user by their email.

### Example Request

```js
const url =
  "https://api.bitrip.com/user?email=thomas.smith.br.testing@gmail.com";

const options = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

### Example Response

```json
{
  "displayName": "Thomas - test",
  "email": "thomas.smith.br.testing@gmail.com",
  "uid": "dn82sDBSlbNj4hY9Hx0m490hIEl2",
  "organisationId": "f11b4f25-4dbe-44a8-a723-bc612eeda4b9",
  "projects": [
    "b549ad06-eb5a-4fa6-88dc-4d80bbc2beae",
    "480aea87-df2e-489d-9829-4daa35a13c02",
    "aae48908-34fc-4d7b-80aa-8d7e4864325a",
    "418b2ae1-a237-46f3-a6ac-4145e7f74aee"
  ]
}
```

If the request is successful, the API will return a JSON response with the requested user's information. The response contains the following properties:

- `displayName`: The user's display name.
- `email`: The user's email address.
- `uid`: The user ID, which is a unique identifier for the user.
- `organisationId`: The unique identifier of the organization that the user is associated with.
- `projects`: An array of project IDs that represent the projects that the user has access to.

Please note that the project IDs included in the response are only provided as examples, and the actual project IDs will be unique strings.

</div>

<div id="label_get_param">
<br>

## `GET: /label/{labelID}`

- Get a label by its ID.

### Example Request:

```js
const url = "https://api.bitrip.com/label/57f1896e-e0f0-458a-bbc2-218d7a557393";

const options = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

### Example Response:

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

- If there is location data from a scan it will be displayed:

```js
{
    "tags": [
        "DES-SA-626200",
        "Task Not Started"
    ],
    "creator_ref": "/users/yEfXdnFr39OwRHeYrdU11aRzPZt1",
    "projectID": null,
    "id": "35c72ec7-71c0-4302-a323-9a411eeee610",
    "modified": {
        "_seconds": 1668478045,
        "_nanoseconds": 679184000
    },
    "created": {
        "_seconds": 1668462842,
        "_nanoseconds": 200000000
    },
    "name": "UTHSC-MAYS-PHASE-3A-LIGHTING-CONTROL-DEVICES",
    "location": "Charleston, SC",
    "latitude": 30.8123753,
    "longitude": -81.9576705
}
```

If the request is successful, the API will return a JSON response with the requested label's information. The response contains the following properties:

- `tags`: An array of tags associated with the label.
- `creator_ref`: A reference to the user who created the label.
- `projectID`: The ID of the project that the label belongs to.
- `id`: A unique identifier for the label.
- `modified`: An object that contains the last modified date and time of the label in seconds and nanoseconds format.
- `created`: An object that contains the creation date and time of the label in seconds and nanoseconds format.
- `name`: The name of the label.
- `location`: The location data associated with a scan, if applicable.
- `latitude`: The latitude of the scan location, if applicable.
- `longitude`: The longitude of the scan location, if applicable.

Please note that the tag values, user reference, and location data included in the response are only provided as examples, and the actual values will be unique strings or coordinates.

</div>
<br/>

<div id="label_post">

## `POST /label`

Creates a new label. Data must be sent as JSON body information.

- `uid` (required) User ID

  - This will set the creator_ref for the label.
  - `String`

- `title` (required)

  - Sets title for label.
  - `String`

- `pid` (optional)

  - Creates label inside of project.
  - `String`

- `tags` (optional)
  - Adds searchable keywords to label.
  - `Array`

### Example Request

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

## Example Response:

If the request is successful, the API will return a JSON response with the requested label's information. The response contains the following properties:

- `tags`: An array of tags associated with the label.
- `creator_ref`: A reference to the user who created the label.
- `projectID`: The ID of the project that the label belongs to.
- `id`: A unique identifier for the label.
- `modified`: An object that contains the last modified date and time of the label in seconds and nanoseconds format.
- `created`: An object that contains the creation date and time of the label in seconds and nanoseconds format.
- `name`: The name of the label.
- `location`: The location data associated with a scan, if applicable.
- `latitude`: The latitude of the scan location, if applicable.
- `longitude`: The longitude of the scan location, if applicable.

</div>

<br>

<div id="label/attachment_post">

## `POST: /label/attachment/{labelID}`

- This endpoint accepts the following parameters:

- `type` (required): Specifies the type of attachment being uploaded. This parameter is mandatory.

  - Accepted options: TEXT
  - Data type: String

- `title`: Specifies the title of the attachment being uploaded. This parameter is optional.

  - Description: A title to give the attachment.
  - Data type: String

- `text` (required): Specifies the body text of the attachment. This parameter is mandatory if `type` is set to `TEXT`.
  - Description: The body text of the attachment.
  - Data type: `String`

### Example Request

```js
fetch(
  "https://api.bitrip.com/label/attachment/41e92e47-133d-4b6a-89d7-a38404ec6024",
  {
    method: "POST",
    headers: {
      "x-api-key": "your api key here",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      type: "TEXT",
      title: "title for attachment",
      text: "text for this attachment",
    }),
  }
)
  .then((res) => res.json())
  .then((labelData) => console.log(labelData))
  .catch((err) => {
    console.log(err);
    console.log(err.response.data.msg);
  });
```

### Example Response

```json
{
  "attachmentId": "id of newly created attachment"
}
```

</div>

<br>

<div id="label/attachment_put">

## `PUT: /label/attachment/{labelID}`

Updates one or more attachments for a label.

The endpoint accepts the following parameters:

- `labelID` (required): The ID of the label to update.
  - Data type: String

The following parameters must be passed in the request body:

- `attachments` (required): An array of attachments to update.
  - Data type: Array
  - Each attachment should have the following properties:
    - `id` (required): The ID of the attachment to update.
      - Data type: String
    - `text` (optional): The updated text for the attachment.
      - Data type: String
    - `title` (optional): The updated title for the attachment.
      - Data type: String

### Example Request

```js
fetch(
  "https://api.bitrip.com/label/attachment/41e92e47-133d-4b6a-89d7-a38404ec6024",
  {
    method: "PUT",
    headers: {
      "x-api-key": "your api key here",
      "Content-Type": "application/json",
    },
    body: JSON.stringify([
      {
        id: attachmentId,
        text: "updated text",
        title: "updated title",
      },
      {
        id: attachmentId2,
        text: "updated text 2",
      },
    ]),
  }
)
  .then((res) => res.json())
  .then((response) => console.log(response))
  .catch((err) => {
    console.log(err.response.data.msg);
  });
```

### Example 2:

```js
const url =
  "https://api.bitrip.com/label/attachment/CFB34486-9335-473A-A986-AB4314D69830";

const requestData = {
  attachments: [
    {
      id: "28e29106-d924-4d64-b422-0a5c8523ccc3",
      title: "New Name",
      text: "body text for attachment",
    },
  ],
};

const options = {
  method: "PUT",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
  body: JSON.stringify(requestData),
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

### Example Response

```json
{
  "msg": "Attachments updated"
}
```

</div>

<br>

<div id="label/attachment_delete">

## `DELETE: /label/attachment/{labelID}`

Deletes one or more attachments associated with a label. The endpoint accepts the following parameters:

- `labelID` (required): The ID of the label from which to delete the attachment(s).
  - Data type: String

The following parameter must be passed in the request body:

- `attachments` (required): An array of attachment IDs to be deleted.
  - Data type: Array of Strings

### Example Request:

```js
fetch(
  "https://api.bitrip.com/label/attachment/41e92e47-133d-4b6a-89d7-a38404ec6024",
  {
    method: "DELETE",
    headers: {
      "x-api-key": "your api key here",
      "Content-Type": "application/json",
    },
    body: JSON.stringify(["attachment id", "another attachment id"]),
  }
)
  .then((res) => res.json())
  .then((response) => console.log(response))
  .catch((err) => {
    console.log(err.response.data.msg);
  });
```

### Example Response:

```json
{
  "msg": "Attachments deleted"
}
```

</div>

<br>

<div id="label_put">

## `PUT: /label/{labelID}`

- This endpoint allows editing of labels by sending the updated value. You can update the following label parameters:

  - uid: Pass a user ID to transfer ownership. If the label is part of a project, the user you are transferring ownership to must also be a part of that project.
    - Data Type: String.
  - tags: Pass items you want the tags array to be set to.
    - Data Type: Array.
  - name: Pass the name you want the label to be updated to.
    - Data Type: String.
  - pid: Pass the project ID you want the label to be set or changed to.
    - Data Type: String.

Example request:

- This request updates the label name and tags

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

### Example Response

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

<div id="label_delete">

## `DELETE: /label/{labelID}`

Delete a label by its ID.

Example Request:

```js
fetch("https://api.bitrip.com/label/946b825f-af94-49c1-8c33-409ef44c758b", {
  method: "GET",
  headers: {
    "x-api-key": "your api key here",
    "Content-Type": "application/json",
  },
})
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((err) => {
    console.error(err);
  });
```

`https://api.bitrip.com/label/946b825f-af94-49c1-8c33-409ef44c758b`

Example Response:

```json
{
  "msg": "Label deleted successfully."
}
```

</div>
<br>

<div id="label/from_project_delete">

## `DELETE: /label/from_project/{labelID}`

- Removes a label from a project, without deleting the label itself.

Example Request:

```js
fetch(
  "https://api.bitrip.com/label/from_project/946b825f-af94-49c1-8c33-409ef44c758b",
  {
    method: "GET",
    headers: {
      "x-api-key": "your api key here",
      "Content-Type": "application/json",
    },
  }
)
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => {
    console.log(err.response.data.msg);
  });
```

Example Response:

```json
{
  "msg": "Label removed from project successfully."
}
```

</div>

<br>

<div id="labels_get">

## `GET: /labels`

- This endpoint retrieves labels based on various optional query parameters. The available parameters are:

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

<div id="projects_get">

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

- `to` (must be used with when) EPOCH timestamp

- `type` (optional) scan type (edited, viewed, scanned)

  - Narrows search to type of interaction with the labels inside of the
    project.

If there is no query params sent it will return all the projects in the
organization.

Example requests:

- Get all projects in organization

`https://api.bitrip.com/projects`

- Get all projects whose labels have been interacted with since Wednesday,
  October 5, 2022 12:19:32 AM

`https://api.bitrip.com/projects?from=1664929172`

- Get all projects that have a label in them with the urgent or done tag

`https://api.bitrip.com/projects?from=1664929172&has_o=urgent,done`

- Get all projects that have labels that have been scanned since October
  5, 2022 12:19:32 AM and has urgent or done tags in them.

`https://api.bitrip.com/projects?from=1664929172&type=scanned&has_o=urgent,done`

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

<div id="post_project">

## `POST /project`

Accepts JSON body data

- uid (required)

  - user ID
  - `String`

- title (required)
  - title for
  - `String`

admins (optional)

- user ids to be assigned as admins
- `Array`

members (optional)

- user ids to be assigned as admins
- `Array`

Example request:

```js
const url = "https://api.bitrip.com/project";

const options = {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
  body: {
    title: "title for project",
    uid: "KFzra4FuAjbQCMawNQ0AlJoGmqv1", // this person will be assigned as the creator, admin and member.
    admins: ["dn82sDBSlbNj4hY9Hx0m490hIEl2"], // will be assigned as an admin and member, has admin privileges.
    members: ["zx90sDBSlbNj4hY9Hx0m490yuikj"], // assigned as member.
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

Example response:

```js
{
    "pid": "9774761e-8e33-46dd-a76f-4be760535bb0"
}
```

</div>

<br>

<div id="delete_project">

## `DELETE: /project{projectId}`

Example request:

```js
const url = "https://api.bitrip.com/project/239847239487-notreal-34903939";

const options = {
  method: "DELETE",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "your api key here",
  },
};

fetch(url, options)
  .then((res) => res.json())
  .then((data) => console.log(data));
```

Example response:

```js
{
  msg: "Project deleted.";
}
```

</div>

<div id="logs_get">

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

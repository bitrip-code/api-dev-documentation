# Routes

BaseRoute: `https://api.bitrip.com`

Security:

Must pass api key in headers behind `x-api-key`

Example:

```js
{
  "x-api-key": "your-api-key-goes-here"
}
```

Routes:

- `/org` - No Args necessary.
- `/user` - Accepts query string, returns Map.
- `/label` - Accepts label id parameter, returns Map.
- `/labels` - Accepts query string, returns Array.
- `/project` - Accepts project id parameter, returns Map.
- `/projects` - Accepts query string, returns Array.
- `/logs` - Accepts query string, returns Array.

<br>

- ! Error
  - Errors are handled with a status 500 code and a `msg` property with error details attached.

## `GET: /org`

Gets org info including member and admin ids for further querying.

Example:

`https://api.bitrip.com/org`

<br>

## `GET: /user`

Gets a user by their ID.

Example:

`https://api.bitrip.com/user?uid=dn82sDBSlbNj4hY9Hx0m490hIEl2`

<br>

## `GET: /label/{labelID}`

Get a single label using a label id after `/label/`

Example:

`https://api.bitrip.com/label/7901dd0d-9a0a-4712-81de-169d572f1491`

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

<br>

## `GET: /project/{projectID}`

Get a single project using a project id after `/project/`

Example:

`https://api.bitrip.com/project/b549ad06-eb5a-4fa6-88dc-4d80bbc2beae`

## `GET: /project`

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
- `lid` (optional) label ID
  - Gets label by its ID
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

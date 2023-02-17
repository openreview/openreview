# Fields

Edits can only have certain fields and have a specific format. Some of these fields are the same as other objects and some are specific to the Edit object.

### id

This is a unique value that identifies the Edit. The `id` of an Edit is generated automatically. It is a random 10 character long string that can contain any of the following values:

```javascript
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```

### cdate

The Creation Date or `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. It usually represents when the Edit was created.

### tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Edit is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

### mdate

The Modification Date or `mdate` shows when the Edit was last modified. The `mdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

### tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Edit is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

### ddate

The Deletion Date or `ddate` is used to soft delete an Edit. This means that Edits with a `ddate` value can be restored but will appear as deleted. The `ddate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

### invitation

The `invitation` field contains the Invitation id used to create or modify the Edit.

### domain

The `domain` is a string that groups all the entities of a venue. Its value is set automatically and cannot be modified.

### signatures

The `signatures` field indicates who created the Edit. Even though the field is an array of strings, only one item can be present in the `signatures` field. Users can sign with different ids depending on their permissions.

### readers

The `readers` field is an array with Group ids that indicates who can retrieve the Edit or see the Edit in the UI.

### nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Edit or see the Edit in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

### writers

The `writers` field is an array with Group ids that indicates who can modify the Edit.

### content

The `content` field has arbitrary fields that can be named as anything with some restrictions. The fields can only contain alphanumeric characters, underscores, and hyphens and they can have a max length of 80 characters.

The fields inside content must contain a value field and optionally a readers field. For example:

```json
{
    "content": {
        "title": {
            "value": "Title"
        },
        "venueid": {
            "value": "OpenReview.net/2023/Conference",
            "readers": [ "OpenReview.net/2023/Conference" ]
        }
    }
}
```

In the example above, readers of the Edit will see that the value of `title` is "Title". However, only members of "OpenReview.net/2023/Conference" will be able to see the value of `venueid`.

### replacement

The `replacement` field is a boolean value that indicates whether the history of changes of the entity that is being edited should restart from the Edit that is being created or modified. In other words, if the value of `replacement` is true, then, the previous Edits for that particular entity are ignored during [inference](inference.md).

### tauthor

The `tauthor` field contains the identity of the user that created the Edge. This property is only present when the user is writer of the Edge.

## Specific Invitation Edit Fields

### invitations

The `invitations` field contains the Invitation id used to create or modify the Edit. Even though the name of the field is in plural, the field contains a string. This is to avoid name collision with the `invitation` field.

### invitation

The `invitation` field contains the [Invitation](../invitation.md) object that will be used during [inference](inference.md) to create or modify the Invitation that will be saved to the database.

## Specific Group Edit Fields

### invitation

The `invitation` field contains the Invitation id used to create or modify the Edit.

### group

The `group` field contains the Group object that will be used during [inference](inference.md) to create or modify the Group that will be saved to the database.

## Specific Note Edit Fields

### invitation

The `invitation` field contains the Invitation id used to create or modify the Edit.

### note

The `note` field contains the [Note](../note/) object that will be used during [inference](inference.md) to create or modify the Note that will be saved to the database.

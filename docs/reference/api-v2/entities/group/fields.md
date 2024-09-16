---
description: >-
  Groups can only have certain fields and have a specific format. Some of these
  fields are the same as other objects and some are specific to the Group
  object.
---

# Fields

## id

This is a unique value that identifies the Group. If its value starts with `~`, it will most likely belong to a profile id (some email addresses contain `~` at the beginning): `~Joe_Doe1`.Email addresses can also have a group in the system. The email itself will be the id of the group. Other groups used for organization and to assign permissions will usually contain `/` to indicate the hierarchy within the venue: `OpenReview.net/Venue_Organizers`.

## members

Groups contain a `members` field contains other group ids. Members of the group are granted the permissions of the group they belong to.

## cdate

The Creation Date or `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. It usually represents when the Group was created.

## tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Group is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## mdate

The Modification Date or `mdate` shows when the Group was last modified. The `mdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Group is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## invitations

The `invitations` field contains all the Invitations that were used to create the Group through the process of inference.

## domain

The `domain` is a string that groups all the entities of a venue. Its value is usually set automatically and cannot be modified.

## signatures

The `signatures` field indicates who created the Note. Even though the field is an array of strings, only one item can be present in the `signatures` field. Users can sign with different ids depending on their permissions.

## readers

The `readers` field is an array with Group ids that indicates who can retrieve the Note or see the Note in the UI.

## nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Note or see the Note in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

## writers

The `writers` field is an array with Group ids that indicates who can modify the Note.&#x20;

## content

The `content` field is a JSON that can contain any field that is specified in the Invitation. The fields in `content` can only contain alphanumeric characters, underscores, and hyphens and they can have a max length of 80 characters. The UI will display by default the value of the field after replacing the underscore characters with spaces and uppercasing the first letter of every word in the field. However, the UI can be instructed to render a different text for each field by defining a string inside `param/fieldName` in the Invitation.

The fields inside content must contain a value field and optionally a readers field. For example:

```json
{
    "id": "or2023",
    "invitations": [ "OpenReview.net/2023/Conference/-/Group" ],
    "readers": [ "everyone" ],
    "writers": [ "~Some_Author1" ],
    "signatures": [ "~Some_Author1" ],
    "content": {
        "somve_value": {
            "value": "Title"
        },
        "another_value": {
            "value": "OpenReview.net/2023/Conference",
            "readers": [ "OpenReview.net/2023/Conference" ]
        }
    }
}
```

{% hint style="info" %}
The same principle applies to any `content` field of any object at any level.
{% endhint %}

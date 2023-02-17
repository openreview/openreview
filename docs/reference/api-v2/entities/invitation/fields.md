# Fields

Invitations can only have certain fields and have a specific format. Some of these fields are the same as other objects and some are specific to the Invitation object.

## id

This is a unique value that identifies the Invitation. Its value, which will always contain the string `/-/` determines the parent group it belongs to as well as the value that is displayed when creating buttons in the UI. For example, the Invitation id `OpenReview.net/Venue_Organizers/-/Submission` belongs to the group `OpenReview.net/Venue_Organizers` and the button displayed in the UI will have the `Submission` value.

## maxReplies

Invitations can be used to create many objects. If you want to restrict the amount of objects created you can set a `maxReplies` integer value. If this value is not present, then any amount of objects can be created using the Invitation.

## minReplies

Invitations are sometimes used for tasks. Until the integer value `minReplies` is met by creating object replying to the Invitation, the task will still be marked as pending. The `minReplies` parameter is only used by the UI.

## cdate

The Creation Date or `cdate` is used to activate an Invitation. The value of a `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. If the `cdate` is set in the future, the Invitation cannot be used until the date set is reached.

## tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Invitation is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## mdate

The Modification Date or `mdate` shows when the Invitation was last modified. The value of a `mdate` is a unix timestamp in milliseconds that can be set either in the past or in the future.

## tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Invitation is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## expdate

The Expiration Date or `expdate` is used to set an expiration date for an Invitation. Once the expiration date is reached, users will not be able to use the Invitation to create new objects, unless the user is a writer of the Invitation. The value of a `expdate` is a unix timestamp in milliseconds that can be set either in the past or in the future.

## duedate

The Due Date or `duedate` is used to set an expiration date for an Invitation in the UI. Its value can be different from the `expdate` and it is never used to validate if the Invitation can still be used. This property is useful to give users during a deadline room when there are network issues due to heavy traffic. Therefore, the `duedate` is usually set with a value less than the `expdate`. The value of a `duedate` is a unix timestamp in milliseconds that can be set either in the past or in the future.

## ddate

The Deletion Date or `ddate` is used to soft delete an Invitation. This means that Invitations with a `ddate` value can be restored but will appear as deleted. The value of a `ddate` is a unix timestamp in milliseconds that can be set either in the past or in the future.

## invitations

The `invitations` field contains all the Invitations that were used to create the Invitation through the process of inference.

## domain

The `domain` is a string that groups all the entities of a venue. Its value is set automatically and cannot be modified.

## signatures

The `signatures` field indicates who created the Invitation. Even though the field is an array of strings, only one item can be present in the `signatures` field. Users can sign with different ids depending on their permissions.

## readers

The `readers` field is an array with Group ids that indicates who can retrieve the Invitation or see the Invitation in the UI.

## nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Invitation or see the Invitation in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

## writers

The `writers` field is an array with Group ids that indicates who can modify the Invitation.&#x20;

## invitees

The `invitees` field is an array with Group ids that indicates who can use the Invitation to create an object. A user can be a reader of an Invitation but not an invitee.

## noninvitees

The `noninvitees` field is an array with Group ids that indicates who cannot use the Invitation. This field is useful in case we want to use a group id in the `invitees` field that contains a member that we want to exclude from the `invitees`. In this case, we can specify the Group id of that user in the `noninvitees` field.

## preprocess

The `preprocess` field contains a string with custom javascript or python code that can be used to add extra validation before an object is created.

## process

The `process` field contains a string with custom javascript or python code that runs after an object is created. This process runs asynchronously and can be used to send emails, for example. Process functions can be executed right after an object is created or with a delay after the object is created.

## dateprocesses

The `dateprocesses` field is an array containing different dates and delays. It can be used to run a specific task after an object is created, on a particular date, or as a cronjob.

## web

The `web` field contains javascript code that runs when the Invitation is loaded in the UI. If the field `web` is not present, a default UI is shown for the Invitation.

## replyForumViews

TODO

## edit

The `edit` field is a JSON that contains the rules and structure for any `edit` that is created using this Invitation. Edits can be for Groups, Notes, and Invitations.

## edge

The `edge` field is a JSON that contains the rules and structure for any `edge` that is created using this Invitation.

## tag

The `tag` field is a JSON that contains the rules and structure for any `tag` that is created using this Invitation.

## content

The `content` field is a JSON that can contain any field with any name defined by the user. Please refer to the [Speficiers](specifiers.md) subsection for more information on how to define the field values.

The `content` field has arbitrary fields that can be named as anything with some restrictions. The fields can only contain alphanumeric characters, underscores, and hyphens and they can have a max length of 80 characters. The UI will display by default the value of the field after replacing the underscore characters with spaces and uppercasing the first letter of every word in the field. However, you can instruct the UI to render a different text for each field by defining a string inside `param/fieldName`. The field `fieldName` can be a string with a max length of 1000 characters and no restriction in terms of what characters can be used.

The fields inside content must contain a value field and optionally a readers field. For example:

```json
{
    "id": "or2023",
    "invitations": [ "OpenReview.net/2023/Conference/-/Submission" ],
    "readers": [ "everyone" ],
    "writers": [ "~Some_Author1" ],
    "signatures": [ "~Some_Author1" ],
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

Everyone can query this Note and see that the value of `title` is "Title". However, only members of "OpenReview.net/2023/Conference" will be able to see the value of `venueid`.

{% hint style="info" %}
The same principle applies to any `content` field of any object at any level.
{% endhint %}

# Fields

Notes can only have certain fields and have a specific format. Some of these fields are the same as other objects and some are specific to the Note object.

## id

This is a unique value that identifies the Note. The `id` of a Note is generated automatically. It is a random 7-14 character long string that can contain any of the following values:

```javascript
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ-_
```

## number

Notes contain a `number` field that is associated to the first Invitation that is used to create the Note. The `number` value is added automatically and it is usually used to give a number id to submissions.

## cdate

The Creation Date or `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. It usually represents when the Note was created.

## tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Note is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## mdate

The Modification Date or `mdate` shows when the Note was last modified. The `mdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Note is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## odate

The Online Date or `odate` is used to show when the Note first becomes public. The `odate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## pdate

The Publication Date or `pdate` is used to show when the Note that represents a submission is marked as Accepted. The `pdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## ddate

The Deletion Date or `ddate` is used to soft delete an Note. This means that Notes with a `ddate` value can be restored but will appear as deleted. The `ddate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## invitation

The `invitation` field contains the invitation id that was used to create the Note.

## signatures

The `signatures` field indicates who created the Note. Even though the field is an array of strings, only one item can be present in the `signatures` field. Users can sign with different ids depending on their permissions.

## readers

The `readers` field is an array with Group ids that indicates who can retrieve the Note or see the Note in the UI.

## nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Note or see the Note in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

## writers

The `writers` field is an array with Group ids that indicates who can modify the Note.&#x20;

## forum

The `forum` field contains a Note id. When the Note `id` field is equal to the `forum` field, it indicates that the Note is a submission. Otherwise, it means that the Note is a comment to the submission. In other words, the `forum` value indicates the forum the Note belongs to.

## replyto

The `replyto` contains a Note id. This field indicates what Note it is replying to. In other words, Notes that contain this field are comments in a forum.

## content

The `content` field is a JSON that can contain any field that is specified in the Invitation. The fields in `content` should preferably only contain alphanumeric characters, underscores, and hyphens and have a max length of 80 characters.

```json
{
    "id": "or2023",
    "invitations": [ "OpenReview.net/2023/Conference/-/Submission" ],
    "readers": [ "everyone" ],
    "writers": [ "~Some_Author1" ],
    "signatures": [ "~Some_Author1" ],
    "content": {
        "title": "Title",
        "venueid": "OpenReview.net/2023/Conference"
    }
}
```

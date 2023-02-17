# Fields

Notes can only have certain fields and have a specific format. Some of these fields are the same as other objects and some are specific to the Note object.

## id

This is a unique value that identifies the Edge. The `id` of an Edge is generated automatically. It is a random 14 character long string that can contain any of the following values:

```javascript
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```

## cdate

The Creation Date or `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. It usually represents when the Edge was created.

## tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Edge is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## mdate

The Modification Date or `mdate` shows when the Edge was last modified. The `mdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Edge is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## ddate

The Deletion Date or `ddate` is used to soft delete an Edge. This means that Edges with a `ddate` value can be restored but will appear as deleted. The `ddate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## signatures

The `signatures` field indicates who created the Edge. Even though the field is an array of strings, only one item can be present in the `signatures` field. Users can sign with different ids depending on their permissions.

## readers

The `readers` field is an array with Group ids that indicates who can retrieve the Edge or see the Edge in the UI.

## nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Edge or see the Edge in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

## writers

The `writers` field is an array with Group ids that indicates who can modify the Edge.&#x20;

## head

The `head` field can contain a Note id or a Group id and connects it to the Note id or Group id defined in the `tail` field. This field is always mandatory and cannot be empty.

## tail

The `tail` field can contain a Note id or a Group id and connects it to the Note id or Group id defined in the `head` field. This field is always mandatory and cannot be empty.

## label

The `label` field contains a string that gives extra information to the Edge. This information can be used to classify Edges and differentiate them from other Edges that have the same Invitation, tail and head values.

## weight

The `weight` field contains a number that defines the weight between two entities defined in `head` and `tail`.

## invitation

The `invitation` field contains the Invitation id that was used as template to create the Edge.

## tauthor

The `tauthor` field contains the identity of the user that created the Edge. This property is only present when the user is writer of the Edge.

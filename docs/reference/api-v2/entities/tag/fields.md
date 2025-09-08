---
description: >-
  Tags can only have certain fields and have a specific format. Some of these
  fields are the same as other objects and some are specific to the Tag object.
---

# Fields

## id

This is a unique value that identifies the Tag. The `id` of an Tag is generated automatically. It is a random 10 character long string that can contain any of the following values:

```javascript
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```

## cdate

The Creation Date or `cdate` is a unix timestamp in milliseconds that can be set either in the past or in the future. It usually represents when the Tag was created.

## tcdate

The True Creation Date or `tcdate` indicates the date in unix timestamp in milliseconds when the Tag is created. Unlike the `cdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## mdate

The Modification Date or `mdate` shows when the Tag was last modified. The `mdate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## tmdate

The True Modification Date or `tmdate` indicates the date in unix timestamp in milliseconds when the Tag is modified. Unlike the `mdate`, its value cannot be set or modified by the user and it is not displayed in the UI.

## ddate

The Deletion Date or `ddate` is used to soft delete an Tag. This means that Tags with a `ddate` value can be restored but will appear as deleted. The `ddate` value is a unix timestamp in milliseconds that can be set either in the past or in the future.

## signature

The `signature` field indicates who created the Tag. Users can sign with different ids depending on their permissions.

## readers

The `readers` field is an array with Group ids that indicates who can retrieve the Tag or see the Tag in the UI.

## nonreaders

The `nonreaders` field is an array with Group ids that indicates who cannot retrieve the Tag or see the Tag in the UI. This field is useful in case we want to use a group id in the `readers` field that contains a member that we want to exclude from the `readers`. In this case, we can specify the Group id of that user in the `nonreaders` field.

## writers

The `writers` field is an array with Group ids that indicates who can modify the Tag.&#x20;

## domain

The `domain` is a string that groups all the entities of a venue. Its value is usually set automatically and cannot be modified.

## label

The `label` field contains a string compliant with the invitation definition.

## weight

The `weight` field contains a number compliant with the invitation definition.

## invitation

The `invitation` field contains the Invitation id that was used as template to create the Tag.

## tauthor

The `tauthor` field contains the identity of the user that created the Tag. This property is only present when the user is writer of the Tag.

## note

The `note` field contains a note id. This field is present when the tag is tagging a Note and its value is used as a pointer.

## profile

The `profile` field contains a profile id. This field is present when the tag is tagging a Profile and its value is used as a pointer.

## group

The `group` field contains a group id. This field is present when the tag is tagging a Group and its value is used as a pointer.

## invitation

The `invitation` field contains a invitation id. This field is present when the tag is tagging an Invitation and its value is used as a pointer.

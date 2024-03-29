# How to hide/reveal fields

As a venue organizer, you may hide fields in a comment or a submission. Comments (decisions, reviews, official comments, public comments, etc.) and submissions are represented by [Notes](../../reference/api-v2/entities/note/) in OpenReview.

#### Hide a field using the readers property

If you want to restrict the visibility of a field to specific users, use the `readers` property. You can use this property to hide fields that **have sensitive information** such as the name of the authors in a submission or a private comment to the PCs.

To apply this feature, simply add the readers property to that particular field in the [Invitation](../../reference/api-v2/entities/invitation.md). The following example defines the `readers` field as a constant, where only the Program Chairs and Senior Area Chairs of a particular submission can see `restricted_field`. Note the use of [Dollar Sign Notation](../../reference/api-v2/entities/invitation/dollar-sign-notation.md) to point to the Senior Area Chairs. Be aware that the dollar sign notation for the submission is different than the replies to the submission forum.

This example is for an additional submission field:

```json
restricted_field: {
  value: {
    param: {
      type: 'string',
      optional: true
    }
  },
  readers: [
    'Your/Venue/ID/Program_Chairs',
    'Your/Venue/ID/Submission${4/number}/Senior_Area_Chairs'
  ]
}
```

The below example could be used for any forum reply, such as reviews, meta reviews, decisions, and comments.

```json
restricted_field: {
  value: {
    param: {
      type: 'string',
      optional: true
    }
  },
  readers: [
    'Your/Venue/ID/Program_Chairs',
    'Your/Venue/ID/Submission${7/content/noteNumber/value}/Senior_Area_Chairs'
  ]
}
```

Please see the [speficiers](../../reference/api-v2/entities/invitation/specifiers.md) section to know the different options you have when defining the `readers` field.

```json
restricted_field: {
  value: {
    param: {
      type: 'string',
      optional: true
    }
  },
  readers: {
    param: {
      regex: '^~.*$|OpenReview\.net\/Conference'
      deletable: true
    }
  }
}
```

Adding the readers property is not sufficient for the field to be hidden. You still need to post an Edit including the value of the readers field. The following change only allows the users in the `readers` field to see `restricted_field`.

```json
restricted_field: {
  value: 'Not for your eyes',
  readers: [ '~Author_One1', '~Author_Two1', 'OpenReview.net/Conference' ]
}
```

In case the [Invitation](../../reference/api-v2/entities/invitation.md) does not allow a field to include a readers field and modifying it is not possible, there are two options around it. You can create a new [Invitation](../../reference/api-v2/entities/invitation.md) whose sole purpose is to allow the readers property to be included in the field or use the [Meta Invitation](../../reference/api-v2/entities/invitation/types-and-structure.md#meta-invitations). Below is an example of what an Edit would look like that hides a field.

```json
{
  invitation: 'OpenReview.net/Conference/-/Edit',
  signatures: [ 'OpenReview.net/Conference' ],
  readers: [ 'OpenReview.net/Conference' ],
  writers: [ 'OpenReview.net/Conference' ],
  note: {
    id: 'noteId1234',
    content: {
      restricted_field: {
        readers: [ '~Author_One1', '~Author_Two1', 'OpenReview.net/Conference' ]
      }
    }
  }
}
```

#### Reveal a field by removing the readers property

If you want to reveal the value of the `restricted_field`, you need to remove the `readers` property from the field. You may do that by passing the `{ delete: true }`. The Invitation needs to allow the field to be deleted with the property `{ deletable: true }` as shown above.

```json
restricted_field: {
  value: 'For your eyes',
  readers: { 'delete': true }
}
```

In case an [Invitation](../../reference/api-v2/entities/invitation.md) does not allow a field to be deleted, there are two options around it. You can create a new [Invitation](../../reference/api-v2/entities/invitation.md) whose sole purpose is to delete the readers property or use the [Meta Invitation](../../reference/api-v2/entities/invitation/types-and-structure.md#meta-invitations). The syntax to remove the `readers` field is the same as the one shown above when using the [Meta Invitation](../../reference/api-v2/entities/invitation/types-and-structure.md#meta-invitations).

```json
{
  invitation: 'OpenReview.net/Conference/-/Edit',
  signatures: [ 'OpenReview.net/Conference' ],
  readers: [ 'OpenReview.net/Conference' ],
  writers: [ 'OpenReview.net/Conference' ],
  note: {
    id: 'noteId1234',
    content: {
      restricted_field: {
        readers: { 'delete': true }
      }
    }
  }
}
```

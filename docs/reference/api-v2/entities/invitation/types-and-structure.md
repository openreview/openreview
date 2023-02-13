# Types and Structure

The fields of an Invitation vary depending on the object that will be created with it. There are 6 different types of invitations:

* [Meta Invitations](types-and-structure.md#meta-invitations)
* [Invitations for Edges](types-and-structure.md#invitations-for-edges)
* [Invitations for Tags](types-and-structure.md#invitations-for-tags)
* [Invitations for Group Edits](types-and-structure.md#invitations-for-group-edits)
* [Invitations for Note Edits](types-and-structure.md#invitations-for-note-edits)
* [Invitations for Invitation Edits](types-and-structure.md#invitations-for-invitation-edits)

## Meta Invitations

Meta Invitations are admin Invitations which are created when the venue is deployed and are available to venue organizers. There is only 1 available per venue and should be used with care and sparingly. Meta Invitations can modify any object in the system that is associated to the venue. Therefore, before using the Meta Invitation, make sure there is no other Invitation that can be used instead. Using an Invitation other than the Meta Invitation is preferred to avoid errors when modifying an object.

{% hint style="info" %}
There is only 1 available per venue and should be used with care and sparingly.
{% endhint %}

{% hint style="info" %}
Meta Invitations can modify any object in the system that is associated to the venue.
{% endhint %}

Meta Invitations have the structure shown below. They can be identified because of the property `edit: true` inside the `invitation` field.

```json
{
  writers: [ "OpenReview.net/Venue_Organizers" ],
  readers: [ "OpenReview.net/Venue_Organizers" ],
  signatures: [ "OpenReview.net" ],
  invitation: {
    id: "OpenReview.net/Venue_Organizers/-/Edit",
    signatures: [ "OpenReview.net" ],
    writers: [ "OpenReview.net" ],
    invitees: [ "OpenReview.net/Venue_Organizers" ],
    readers: [ "OpenReview.net/Venue_Organizers" ],
    edit: true
  }
}
```

## Invitations for Edges

Edge Invitations are used to create Edges and contain the `edge` field that defines the template for the Edges. This means that when creating an Edge object with the Invitation below, the rules and structure defined inside the `edge` field will need to be followed.

{% hint style="warning" %}
Do NOT copy the values from this example in your venue. This example is a simplified version of an Edge Invitation and it is only used to understand its structure.
{% endhint %}

```json
{
  id: "OpenReview.net/Venue_Organizers/-/Bid",
  readers: [ "everyone" ],
  invitees: [ "OpenReview.net/Reviewers" ],
  writers: [ "OpenReview.net/Venue_Organizers" ],
  signatures: [ "OpenReview.net/Venue_Organizers" ],
  edge: {
    id: {
      param: {
        withInvitation: "OpenReview.net/Venue_Organizers/-/Bid",
        optional: true
      }
    },
    readers: { param: { regex: "~.*" } },
    writers: { param: { regex: "~.*" } },
    signatures: { param: { regex: "~.*" } },
    head: { param: { type: "note" } },
    tail: { param: { type: "profile" } },
    label: { param: { regex: ".*" } }
  }
}
```

## Invitations for Tags

Tag Invitations are used to create Tags and contain the `tag` field that defines the template for the Tags. This means that when creating a Tag object with the Invitation below, the rules defined and structure inside the `tag` field will need to be followed.

{% hint style="warning" %}
Do NOT copy the values from this example in your venue. This example is a simplified version of a Tag Invitation and it is only used to understand its structure.
{% endhint %}

```json
{
  id: "OpenReview.net/Venue_Organizers/-/Tag",
  signatures: [ "OpenReview.net/Venue_Organizers" ],
  writers: [ "OpenReview.net/Venue_Organizers" ],
  invitees: [ "~" ],
  readers: [ "everyone" ],
  tag: {
    readers: [ "everyone" ],
    signatures: { param: { regex: ".+" } },
    writers: { param: { regex: ".+" } },
    nonreaders: { param: { regex: ".*", optional: true } },
    tag: { param: { minLength: 1, maxLength: 100 } }
  }
}
```

## Invitations for Group Edits

Group Edit Invitations are used to create Group Edits and contain the `edit` field that defines the template for the Group Edits. This means that when creating a Group Edit object with the Invitation below, the rules and structure defined inside the `edit` field will need to be followed.

{% hint style="warning" %}
Do NOT copy the values from this example in your venue. This example is a simplified version of a Group Edit Invitation and it is only used to understand its structure.
{% endhint %}

```json
{
  id: "OpenReview.net/Venue_Organizers/-/Group",
  signatures: [ "OpenReview.net/Venue_Organizers" ],
  writers: [ "OpenReview.net/Venue_Organizers" ],
  invitees: [ "OpenReview.net/Venue_Organizers" ],
  readers: [ "OpenReview.net/Venue_Organizers" ],
  edit: {
    readers: [ "OpenReview.net/Venue_Organizers" ],
    signatures: { param: { regex: ".+" } },
    writers: { param: { regex: ".*" } },
    group: {
      id: "OpenReview.net/Reviewers",
      readers: [ "OpenReview.net/Venue_Organizers" ],
      signatures: { param: { regex: ".+" } },
      signatories: { param: { regex: ".+", optional: true } },
      writers: { param: { regex: ".*" } },
      members: { param: { regex: ".*", optional: true } }
    }
  }
}
```

## Invitations for Note Edits

Note Edit Invitations are used to create Note Edits and contain the `edit` field that defines the template for the Note Edits. This means that when creating a Note Edit object with the Invitation below, the rules and structure defined inside the `edit` field will need to be followed.

{% hint style="warning" %}
Do NOT copy the values from this example in your venue. This example is a simplified version of a Note Edit Invitation and it is only used to understand its structure.
{% endhint %}

```json
{
  id: "OpenReview.net/Venue_Organizers/-/Note",
  signatures: [ "OpenReview.net/Venue_Organizers" ],
  writers: [ "OpenReview.net/Venue_Organizers" ],
  invitees: [ "~" ],
  readers: [ "everyone" ],
  edit: {
    readers: [ "everyone" ],
    signatures: { param: { regex: ".+" } },
    writers: { param: { regex: ".*" } },
    note: {
      readers: [ "everyone" ],
      signatures: { param: { regex: ".+" } },
      writers: { param: { regex: ".*" } },
      content: {
        title: {
          value: { param: { regex: ".*" } }
        }
      }
    }
  }
}
```

## Invitations for Invitation Edits

Invitation Edit Invitations are used to create Invitation Edits and contain the `edit` field that defines the template for the Invitation Edits. This means that when creating an Invitation Edit object with the Invitation below, the rules and structure defined inside the `edit` field will need to be followed.

Invitations that are used to create Invitation Edits are the most complex ones. Theoretically, they can have any depth, but you will probably hit restrictions if you try to create a very big Invitation.

{% hint style="warning" %}
Do NOT copy the values from this example in your venue. This example is a simplified version of an Invitation Edit Invitation and it is only used to understand its structure.
{% endhint %}

```json
{
  id: "OpenReview.net/Venue_Organizers/-/Invitation",
  signatures: [ "OpenReview.net/Venue_Organizers" ],
  writers: [ "OpenReview.net/Venue_Organizers" ],
  invitees: [ "~" ],
  readers: [ "everyone" ],
  edit: {
    signatures: { param: { regex: ".+" } },
    content: {
      title: { value: { param: { type: "string", regex: ".*" } } },
      duedate: { value: { param: { type: "integer"  } } }
    },
    readers: [ "OpenReview.net/Venue_Organizers", "${2/signatures}" ],
    writers: [ "OpenReview.net/Venue_Organizers" ],
    invitation: {
      id: "OpenReview.net/-/Submission",
      signatures: [ "${3/signatures}" ],
      readers: [ "~" ],
      writers: [ "OpenReview.net/Venue_Organizers", "${3/signatures}" ],
      invitees: [ "${3/content/invitees/value}" ],
      duedate: "${2/content/duedate/value}",
      content: {
        optional: { value: { param: { type: "string", optional: true } } }
      },
      edit: {
        signatures: { param: { regex: ".*" } },
        readers: [ "OpenReview.net/Venue_Organizers", "${2/signatures}" ],
        writers: [ "OpenReview.net/Venue_Organizers", "${2/signatures}" ],
        note: {
          signatures: [ "${3/signatures}" ],
          readers: [ "OpenReview.net/Venue_Organizers", "${3/signatures}" ],
          writers: [ "OpenReview.net/Venue_Organizers", "${3/signatures}" ],
          content: {
            title: {
              value: {
                param: {
                  type: "string",
                  regex: "${8/content/title/value}"
                }
              }
            },
            authors: {
              value: { param: { type: "string[]", regex: ".*", optional: true } },
              readers: [ "OpenReview.net/Venue_Organizers", "${5/signatures}" ]
            },
            authorids: {
              value: { param: { type: "group[]", regex: ".*", optional: true } },
              readers: [ "OpenReview.net/Venue_Organizers", "${5/signatures}" ]
            }
          }
        }
      }
    }
  }
}
```

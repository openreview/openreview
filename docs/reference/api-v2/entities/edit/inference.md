# Inference

The process of inference combines Edits to create an entity before saving it to the database. In other words, entities such as [Invitations](../invitation.md), Groups or [Notes](../note/) cannot be created directly, they need to be created through Edits and the process of inference.

Edits are sorted in a fixed chronological order using the `tcdate` field during inference. Since the `tcdate` value can never be modified, Edits can never change their position in the history of changes. However, the history of changes can be modified by destructively changing an Edit. This is only possible if the user has writer permissions on the Edit.

The process of inference can add, remove and replace fields **as long as the Invitation used to create the Edit allows it**. To illustrate this, we will focus on inference used to create Notes, however, the same rules apply to Invitations and Groups.

For the following examples, assume that the following [Invitation](../invitation.md) is used:

```javascript

{
  id: 'OpenReview.net/Conference/-/Submission',
  signatures: [ 'OpenReview.net' ],
  writers: [ 'OpenReview.net' ],
  invitees: [ '~' ],
  readers: [ 'everyone' ],
  domain: 'OpenReview.net/Conference'
  edit : {
    signatures: { param: { regex: '.+' } },
    readers: [ 'OpenReview.net/Conference', '${2/signatures}' ],
    writers: [ 'OpenReview.net/Conference' ],
    replacement: { param: { 'enum': [ true, false ], optional: true } },
    note: {
      id: {
        param: {
          withInvitation: 'OpenReview.net/Conference/-/Submission',
          optional: true
        }
      },
      signatures: [ '${3/signatures} ],
      readers: [ 'everyone' ],
      writers: [ 'OpenReview.net/Conference', '${3/signatures}' ],
      content: {
        title: {
          value: {
            param: {
              type: 'string',
              optional: true
            }
          }
        },
        abstract: {
          value: {
            param: {
              type: 'string',
              optional: true,
              deletable: true
            }
          },
          readers: {
            param: {
              regex: '^~.*$|OpenReview\.net\/Conference'
              deletable: true
            }
          }
        },
        authors: {
          value: { param: { type: 'string[]', minLength: 1, optional: true } },
          readers: [ 'OpenReview.net/Conference', '${5/signatures}' ]
        },
        authorids: {
          value: { param: { type: 'group[]', regex: '^~.+', optional: true } },
          readers: [ 'OpenReview.net/Conference', '${5/signatures}' ]
        }
      }
    }
  }
}
```

### Creating Notes

Suppose the following Note Edit is created:

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference'
  note: {
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      title: {
        value: 'Title'
      },
      authors: {
        value: [ 'Author One' ],
        readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
      },
      authorids: {
        value: [ '~Author_One1' ],
        readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
      }
    }
  }
}
```

Since this is the first an only Edit the resulting Note will only contain the fields and values inside the `note` field of the Edit.

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Title'
    },
    authors: {
      value: [ 'Author One' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authorids: {
      value: [ '~Author_One1' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    }
  }
}
```

You may have noticed the fields `id`, `invitations` and `domain` that were not included in the Edit but still appear after the inference runs. These fields are set automatically and cannot be manually modified. The `invitations` field shows all the different Invitations that have contributed to the modification of the Note. The field `domain` shows the venue that the Note belongs to and it is present in every single object created.

### Adding Fields

Let's say that we now want to add the field abstract to the Note. We can create a new Note Edit:

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  note: {
    id: 'mIcD4PJBaU',
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      abstract: {
        value: 'Abstract',
        readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
      }
    }
  }
}
```

Notice that we now pass the field `note.id` to the Edit. This indicates what Note we want to modify. The resulting Note will then contain the `abstract` field:

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Title'
    },
    abstract: {
      value: 'Abstract',
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authors: {
      value: [ 'Author One' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authorids: {
      value: [ '~Author_One1' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    }
  }
}
```

### Replacing Field Values

Values can be overwritten by passing the same field with a new value. For example, if we want to update the abstract field that we just added, we can create the following Note Edit

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  note: {
    id: 'mIcD4PJBaU',
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      abstract: {
        value: 'Revised Abstract'
      }
    }
  }
}
```

The resulting Note will look like this:

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Title'
    },
    abstract: {
      value: 'Revised Abstract'
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authors: {
      value: [ 'Author One' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authorids: {
      value: [ '~Author_One1' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    }
  }
}
```

### Removing Fields

In order to remove a field from a Note, we can pass the object `{ delete: true }` as value to the field.

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  note: {
    id: 'mIcD4PJBaU',
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      abstract: {
        readers: { 'delete': true }
      }
    }
  }
}
```

The resulting Note will look like this:

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Title'
    },
    abstract: {
      value: 'Revised Abstract'
    },
    authors: {
      value: [ 'Author One' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authorids: {
      value: [ '~Author_One1' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    }
  }
}
```

In this case the abstract now inherits the readers of the Note, just like title. The `authors` and `authorids` fields are still only visible to a subset of people.

You can completely remove the abstract by now deleting the `value` field:

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  note: {
    id: 'mIcD4PJBaU',
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      abstract: {
        value: { 'delete': true }
      }
    }
  }
}
```

The resulting Note now looks like this:

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Title'
    },
    authors: {
      value: [ 'Author One' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    },
    authorids: {
      value: [ '~Author_One1' ],
      readers: [ 'OpenReview.net/Conference', '~Author_One1' ]
    }
  }
}
```

The Note will now look exactly the same as the first Note that was created from the first Edit. The difference is that we can see the history of changes by querying the Edits associated to the Note.

### Destructive Changes

Edits can be modified directly and alter the history of changes. This can achieved by passing the `id` of the Edit that we want to modify like so:

```javascript
{
  id: 'oORBZnuLKi'  // This is the id of the Edit that we want to modify. 
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ 'OpenReview.net/Conference' ],
  readers: [ 'OpenReview.net/Conference' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  note: {
    id: 'mIcD4PJBaU',
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      abstract: {
        value: 'A different Abstract'
      }
    }
  }
}
```

### Replacement

The field `replacement` can be used when we want to restart the history of changes. In other words, Edits created before the one that contains `replacement: true` will be ignored during inference. The ignored Edits will still exist but will not be considered for the resulting values of the entity. Consider the following Edit:

```javascript
{
  invitation: 'OpenReview.net/Conference/-/Submission',
  signatures: [ '~Author_One1' ],
  readers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  writers: [ 'OpenReview.net/Conference' ],
  domain: 'OpenReview.net/Conference',
  replacement: true,
  note: {
    id: 'mIcD4PJBaU',
    signatures: [ '~Author_One1' ],
    readers: [ 'everyone' ],
    writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
    content: {
      title: {
        value: 'Replacement Title'
      }
    }
  }
}
```

The resulting Note looks like this:

```javascript
{
  id: 'mIcD4PJBaU'
  invitations: [ 'OpenReview.net/Conference/-/Submission' ],
  signatures: [ '~Author_One1' ],
  readers: [ 'everyone' ],
  writers: [ 'OpenReview.net/Conference', '~Author_One1' ],
  domain: 'OpenReview.net/Conference',
  content: {
    title: {
      value: 'Replacement Title'
    }
  }
}
```

As you can see, the Edit containing the `replacement: true` is considered to be the first Edit during inference. If a subsequent Edit contains `replacement: true` then that Edit will considered to be the first Edit and the previous Edits will be ignored.
